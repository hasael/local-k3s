# K8s auto update script
To keep the local k3s infrastructure up to date and integrated with the various CI , we can utilize an [[S3]] folder to store the current state.
How it works in a nutshell
- Have the k8s `yaml` files on the solution you intend to deploy (preferable same folder)
- During the CI of that/those solutions, store all k8s `yaml` files on an S3 bucket
- Additionally, store the latest commit tag on a file named `latest_commit`
- From the master node, we run a script every X minutes using crontab
- The script will
	- sync local folder with the S3 bucket 
	- Check if the value on `latest_commit` file matches the value on `previous_commit` file (which will be a new one the first time). In case they dont match, we apply the changes using kubectl and set the 'prev_commit' file value to the 'latest_commit'

### Ensure latest image is pulled
K8s does not ensure the latest image with the same tag is pulled on every `apply`.
To ensure this we should add `imagePullPolicy: Always` under template.spec.containers on the k8s yaml file.
Additionally, `kubectl rollout restart` ensures all pods on a deployment are restarted.
Hence, every time we run this command, the latest image is pulled.
	

### Update script
In case we have `latest` tag on our images, the pod container is not updated. 
We can set up a script to ensure that. On the master node.

```bash
#!/bin/bash
sudo aws s3 sync s3://hasael-local-k3s-state /home/pi/k3s-state/
last_commit=$(cat /home/pi/k3s-state/last_commit)
prev_commit=$(cat /home/pi/k3s-state/prev_commit)
if [ "$last_commit" != "$prev_commit" ]
then
 sudo kubectl apply -f /home/pi/k3s-state/
 sudo kubectl rollout restart -f /home/pi/k3s-state/
 echo $last_commit \> /home/pi/k3s-state/prev_commit
fi
```
Create an empty 'prev_commit' file with any value

### Set up in crontab
```
sudo crontab -e
* * * * * /home/pi/updatek3s >/home/pi/updatek3s.logs 2>&1
```

### CI
During the CI pipeline, update the S3 bucket file 'last_commit' with the last commit hash and sync all k8s files.
```bash
sudo apt-get install -y awscli
aws configure set aws_access_key_id ${{ secrets.AWS_KEY_ID }}
aws configure set aws_secret_access_key ${{ secrets.AWS_ACCESS_KEY }}
aws configure set region eu-west-1
aws s3 sync ./k8s/ ${{ secrets.K8S_STATE_BUCKET }}
```


---
#raspberrypi #k8s  #localcluster 