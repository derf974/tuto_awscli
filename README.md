
# INSTALLATION
```
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
python get-pip.py --user
pip install awscli --user
```

# CONFIGURATION 
```
aws configure
```
# Pour EC2 
## AFFICHAGE 
```
aws ec2 describe-regions --output table
```
## CREATION ET CONFIGURATION SECURITY GROUP
```
aws ec2 create-security-group --group-name devenv-sg --description "security group for development environment in EC2"

aws ec2 authorize-security-group-ingress --group-name devenv-sg --protocol tcp --port 22 --cidr 0.0.0.0/0
```

## CREATION DE LA CLE SSH
```
aws ec2 create-key-pair --key-name devenv-key --query 'KeyMaterial' --output text > devenv-key.pem

chmod 400 devenv-key.pem
```

## START INSTANCE AND RETURN INSTANCE ID
```
aws ec2 run-instances --image-id ami-6e1a0117 --security-group-ids sg-b018ced5 --count 1 --instance-type t2.micro --key-name devenv-key --query 'Instances[0].InstanceId'
```

## GET IP ADDR
```
aws ec2 describe-instances --instance-ids "i-0787e4282810ef9cf" --query 'Reservations[0].Instances[0].PublicIpAddress'

ssh -i devenv-key.pem ubuntu@${IP_INSTANCE}
```

## STOP INSTANCE
```
aws ec2 stop-instances --instance-ids "i-0692330310e72436a"
```

## RESILIER/TERMINER L'INSTANCE
```
aws ec2 terminate-instances --instance-ids "i-0692330310e72436a"
```
