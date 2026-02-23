'# 203669-rds-cloud-iac  - RDS-Tool\n\nThis project Provisions RDS Instances to Shared (a206645) Namespace.\n\n## Prerequisites\n\n- [Python](https://realpython.com/installing-python/)\n
- Version 3.7 is currently the recommended version for use by cloud-iac.\n- cloud-iac\n  - [Install the latest version of cloud-iac](https://github.com/tr/nuvola_cloud-iac/blob/v2/docs/ins
tallation/installation.md) on your local workstation.\n  - Review the [cloud-iac Quick Start](https://github.com/tr/nuvola_cloud-iac/blob/v2/docs/quickstart/quickstart.md) guide if you are
 new to cloud-iac.\n- [git](https://github.com/git-guides/install-git)\n  - Install the most recent version of git for your platform.\n- Access to AWS\n  - We recommend running the CodePip
eline from a CI/CD account and targeting a BU account for creating Aurora clusters.\n- CodeStar GitHub connection\n  - A CodeStar GitHub connection must exist in the AWS account from which
 the CodePipeline will run.\n  - Reference [this document](https://github.com/tr/nuvola_hub-docs/blob/main/aws-cicd/codepipeline-github.md) to determine which CodeStar GitHub connection to
 use, or, if one does not exist, request a new CodeStar GitHub connection by following the steps in the document.\n\n## Getting Started\n\nThe following are the steps required to set up a
new copy of this project.\n\n1. Create new GitHub repository.\n2. Clone [RDS-Tool](https://github.com/tr/nuvola_rds-tool) repository to your desired local folder.\n3. Open and modify the f
ollowing config files for your use case.\n    - variables.yaml\n    - config/config.yaml\n    - config/prod/config.yaml\n\nNote: Change the Directory structure according to your project/en
vironment naming standards..\n\n4. Platform Engineering recommends deploying the pipeline from a CI-CD AWS account. When deploying from CI-CD, set the *iam_role* variable in the *config/de
v/config.yaml* configuration file with the ARN of the PowerUser2 role in the target BU account.\n  - Example of configuring the pipeline to use a PowerUser2 role in a BU AWS account:\n   `
``yaml\n   iam_role : arn:aws:iam:<BU aws account id>:role/human-role/<a{AssetInsightId}>-PowerUser2\n   ```\n5. After modifications are complete login to the correct account using `cloud-
tool login`, then deploy the pipeline using `cloud-iac deploy-pipeline`.\n6. *(Not recommended)* If you prefer to deploy from a BU AWS account instead, then you will need to manually creat
e an IAM service role as follows:\n   - Use the CloudFormation template that was generated in the following location *cf-pipeline/deployment-iam-role.json* to create a new stack for the ne
w IAM role.\n   - Set the value of the *iam_role* variable in the *config/dev/config.yaml* configuration file with the ARN of the role that you just created. \n   - Example of configuring
the pipeline to use the manually created IAM service role in a BU AWS account:\n   ```yaml\n   iam_role : arn:aws:iam:<BU aws account id>:role/service-role/<a{AssetInsightId}-{ProjectCode}
>-deploy\n   ```\n   - After modifications are complete, redeploy the pipeline using `cloud-iac deploy-pipeline`.\n7. Once deployed, push to the newly created repository using the followin
g commands:\n```\ngit add .\ngit commit -m "your commit msg here"\ngit push -u origin main\n```\n8. The pipeline will execute then trigger a CodeBuild project.\n\n## Postgres Extensions \n
\nThese instructions detail how to install Postgres extensions for your Aurora database cluster. \n\nInstall the latest version of rds_tool to your local workstation using pip. \n\n```shel
l\npip install rds-tool --upgrade\n```\n\nAfter installing the rds_tool package in your local machine use cloud-tool to log in into the AWS account in which your Aurora databse cluster exi
sts. After successfully loggin in to the AWS account, use cloud-tool\'s\ntunneling functionality to establish a connection to the cluster.\n\nNote: The cluster to which you\'re connecting
must have the Postgresql Bastion security group attached to it.\n\n\n```shell\n## cloud tool tunneling\ncloud-tool ssh-tunnel -I -c <cluster writer endpoint>  -r 5432\n```\n\nAfter success
fully tunnelling to your cluster, use rds-tool\'s ```pg-extensions``` CLI command to create extensions against the target cluster.\n\nWe recommend installing the following extensions:\n- p
gaudit\n- uuid-ossp\n- pg_partman\n\nThe following are the required parameters for the ``pg-extensions`` command:\n```shell\npg-extensions <ext 1>,<ext 2> ... <ext n> -c <cluster-name> -s
<secret-name> -r <aws-region>\n````\nAlternatively, you can set the cluster name and secret name via environment variables: \n\n```shell\nexport CLUSTER_NAME=<cluster-name>\nexport SECRET_
NAME=<secret-name>\n\npg-extensions <ext 1>,<ext 2> ... <ext n>\n```\n\nExample:\n```shell\npg-extensions pgaudit,uuid-ossp,pg_partman -c a204503-rds-tool-cloud-iac-demo-ok-to-delete -s a2
04503-rds-tool-cloud-iac-demo-ok-to-delete -r us-east-1\n```\n\n\n## RDS Tool Configs\nThis pipeline is designed to ingest YAML configuration files. These YAML files will inform RDS-Tool a
s to which AWS resources to provision. These configuration files need to be stored inside the rds_tool_config/{environment}/{config.yaml} folder. The environments must match the env parame
ter set in the database.yaml file. Example config files can be found in the [rds_tool_configs/examples](https://github.com/tr/nuvola_rds-tool-cloud-iac/tree/main/rds_tool_configs/examples)
 folder.\n\n\n## FAQ\n\nView the [FAQ](https://github.com/tr/nuvola_rds-tool-cloud-iac/wiki/FAQ) for answers to frequently asked questions.\n\n## More Resources\nSee the [wiki](https://git
hub.com/tr/nuvola_rds-tool-cloud-iac/wiki)  for more details.\n\nThis project utilizes the [RDS-Tool library](https://github.com/tr/nuvola_rds-tool/). More details about RDS-Tool and what
it supports are available in the [RDS-Tool Wiki](https://github.com/tr/nuvola_rds-tool/wiki).\n.\n#\x00 \x00a\x002\x000\x003\x008\x004\x008\x00_\x00c\x00h\x00e\x00c\x00k\x00p\x00o\x00i\x00
n\x00t\x00c\x00m\x00d\x00b\x00-\x00r\x00d\x00s\x00-\x00i\x00a\x00c\x00-\x00p\x00r\x00e\x00p\x00r\x00o\x00d\x00\r\x00\n\x00'
