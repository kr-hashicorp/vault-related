# vault-related

소스 코드 암호화는 다음과 같은 절차로 진행.
- 암/복호화 엔진 활성화
- 암/복호화 키 생성
- 소스 코드를 base64로 인코딩 후 암호화
- 암호화된 소스를 복호화 후 base64로 디코드

소스 코드에 대한 암복화를 위한 명령어는 다음과 같습니다.

## 암/복호화 엔진 활성화
<code>
  $ vault secrets enable transit
</code>

## 암/복호화 키 생성
<code>
$ vault write -f transit/keys/code_java
</code>

## 소스 코드 암호화
데모 목적으로 암호화 파일을 생성.

<code>
$ vault write transit/encrypt/code_java plaintext=$(base64 -i TransitUtil.java) <p>
$ vault write --field=ciphertext transit/encrypt/code_java plaintext=$(base64 -i TransitUtil.java) > TransitUtil.java_ciphertext
</code>

## 소스 코드 복호화
데모 목적으로 base64로 인코드된 복호화 파일 생성

<code>
$ vault write transit/decrypt/code_java ciphertext=@TransitUtil.java_ciphertext 
  </code>
<p> 
 <code>
$ vault write -field=plaintext transit/decrypt/code_java ciphertext=@TransitUtil.java_ciphertext > TransitUtil.java_base64_text 
 </code><p>
 <code>
$ base64 -i TransitUtil.java_base64_text -o TransitUtil.java_decode --decode
</code>
