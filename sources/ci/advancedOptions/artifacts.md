# Build artifacts
Shippable does not store artifacts for your builds. You will need to handle uploading artifacts as part of your configuration in your shippable.yml.

For example, the sample snippet below copies the entire build directory to an AWS S3 bucket which is in the region us-east-1:

```
env:
  global:
    #secure variable contains values for AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
    - secure: HKwYujx/qmsyQQdHvR2myu8HLUDtcLeDyYV149YJuxIV4J7Hk3SxeY8X3D6aTlR8mvMnd/ZFY+tGNUh4G0xtLLjjZcPsBgvFlB

build:

  on_success:
    - aws s3 sync $SHIPPABLE_BUILD_DIR "s3://bucket_name" --region "us-east-1"


```

Similarly, you can copy contents of any folder with `$SHIPPABLE_BUILD_DIR/folder_name` depending on where the artifacts are for your build.
If you need help defining secure variables, you can check out [our instructions](environmentVariables/#secure-variables)
