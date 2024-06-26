## Lambda Function

```python
import json
import boto3

def get_volume_ID(volume_arn):
    arn_parts = volume_arn.split(':')
    volume_id = arn_parts[-1].split('/')[-1]
    return volume_id

def lambda_handler(event, context):
    try:
        volume_arn = event['resources'][0]
        volume_id = get_volume_ID(volume_arn)

        ec2_client = boto3.client('ec2')

        response = ec2_client.modify_volume(
            VolumeId=volume_id,
            VolumeType='gp3'
        )

        return {
            'statusCode': 200,
            'body': json.dumps('Volume type modified successfully!')
        }
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f'Error modifying volume type: {str(e)}')
        }
