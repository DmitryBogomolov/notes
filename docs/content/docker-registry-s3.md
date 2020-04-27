# Docker Registry with S3

Run docker registry in container with data stored in *S3* bucket.

Create *s3://my-test-docker-storage* (use actual name) bucket with the following policy.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1504612342000",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation",
                "s3:ListBucketMultipartUploads"
            ],
            "Resource": [
                "arn:aws:s3:::my-test-docker-storage"
            ]
        },
        {
            "Sid": "Stmt1504612484000",
            "Effect": "Allow",
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:DeleteObject",
                "s3:GetObject",
                "s3:ListMultipartUploadParts",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::my-test-docker-storage/*"
            ]
        }
    ]
}
```

Create *config.yml* for the registry.
```yml
version: 0.1
log:
  fields:
    service: registry
storage:
  s3:
    accesskey: ABCDEFGHIJKLMNOPQRST # use actual ACCESS_KEY
    secretkey: 1234567891234567891234567891234567891234 # use actual SECRET_ACCESS_KEY
    region: us-east-1 # use actual REGION
    bucket: my-test-docker-storage # use actual bucket name
  cache:
    blobdescriptor: inmemory
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
```

Run container.
```bash
docker run \
    --name my-docker-registry \
    --restart always \
    -d \
    -p 5000:5000 \
    -v $(pwd)/config.yml:/etc/docker/registry/config.yml:ro \
    registry:<tag>
```

Inspect bucket.
```bash
aws s3 ls --summarize --human-readable --recursive s3://my-test-docker-storage
```
