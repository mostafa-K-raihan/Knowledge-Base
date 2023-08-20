```
alias aws_login_admin="aws-sso-util login --profile admin"

alias aws_ec2_start="aws_login_admin && AWS_PROFILE=admin aws ec2 start-instances --instance-ids i-0d6b4178329c22f84"

alias aws_ec2_stop="aws_login_admin && AWS_PROFILE=admin aws ec2 stop-instances --instance-ids i-0d6b4178329c22f84"

alias aws_ec2_check="aws_login_admin && AWS_PROFILE=admin aws ec2 describe-instances --instance-ids i-0d6b4178329c22f84 --query 'Reservations[].Instances[].State.Name'"
```
