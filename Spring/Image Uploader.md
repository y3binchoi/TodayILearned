# [Spring] Image Uploader
>하나의 프로젝트마다 소개 이미지를 넣어줘야 했다. 이미지를 form-data로 받아서 AWS S3에 업로드하는 코드를 작업 순서대로 정리해봤다.

###### TL;DR
1. AWS S3 버킷 생성
2. spring project에 연결 설정
###### 작업 환경
```
IntelliJ
Spring Boot 2.7.2
Java 17
gradle
```

## 설정
### build.gradle
```java
// aws s3  
implementation platform('com.amazonaws:aws-java-sdk-bom:1.11.228')  
implementation 'com.amazonaws:aws-java-sdk-s3'
```
###  application-aws.yml
```java
cloud:
  aws:
    credentials:
      access-key: # IAM key
      secret-key: # IAM key
    s3: 
      bucket: # 버킷 이름 (아시아-서울: ap-northeast-2)
    region: 
      static: # S3 지역
    stack:
      auto: false
```
## 코드
### AwsS3Config.class
액세스 키로 AWS S3에 접근할 수 있도록 설정한다.
```java
import com.amazonaws.auth.AWSStaticCredentialsProvider;  
import com.amazonaws.auth.BasicAWSCredentials;  
import com.amazonaws.services.s3.AmazonS3Client;  
import com.amazonaws.services.s3.AmazonS3ClientBuilder;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class AwsS3Config {  
  
    @Value("${cloud.aws.credentials.access-key}")  
    private String accessKey;  
  
    @Value("${cloud.aws.credentials.secret-key}")  
    private String secretKey;  
  
    @Value("${cloud.aws.region.static")  
    private String region;  
  
    @Bean  
    public BasicAWSCredentials AwsCredentials() {  
        return new BasicAWSCredentials(accessKey, secretKey);  
    }  
  
    @Bean  
    public AmazonS3Client AwsS3Client() {  
  
        return (AmazonS3Client) AmazonS3ClientBuilder.standard()  
                .withRegion(region)  
                .withCredentials(new AWSStaticCredentialsProvider(this.AwsCredentials()))  
                .build();  
    }  
}
```
- 보안이 필요한 정보들을 코드에 직접 적지 않고 yml 파일에 저장한 후 @Value로 가져온다.
### Entity


###### 참고
- [AWS - 스프링부트 & S3에 이미지 업로드하기](https://velog.io/@rainbowweb/AWS-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-S3)
- 