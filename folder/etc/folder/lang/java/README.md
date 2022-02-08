
# keyword

1. Transient
2. volatile
3. synchronized
4. Thread
   1. start
   2. join
   3. stop
   4. sleep
   5. interrupt
   6. yield
   7. wait
   8. notify
   9. notifyAll




Entity List To DTO List
```java
List<SiteInfoDTO> siteList = dataList.stream().map(SiteInfo::entityToDTO).collect(Collectors.toList());
```

entity page to dto page
```java
Page<ObjectDto> entities = objectEntityRepository.findAll(pageable).map(ObjectDto::fromEntity);
```

시간 
https://wickso.me/java/java-8-date-time/
https://www.daleseo.com/java8-zoned-date-time/
https://java119.tistory.com/52
https://madplay.github.io/post/java8-date-and-time
https://www.baeldung.com/java-day-start-end
https://jaimemin.tistory.com/1537

인텔리제이 코드컨벤션 설정
https://055055.tistory.com/97
https://naver.github.io/hackday-conventions-java/


## java base64 url safe encode/decode
```java
Base64.Encoder enc = Base64.getEncoder();
Base64.Encoder encURL = Base64.getUrlEncoder();

byte[] bytes = enc.encode("subjects?_d".getBytes());
byte[] bytesURL = encURL.encode("subjects?_d".getBytes());

System.out.println(new String(bytes)); // c3ViamVjdHM/X2Q=      notice the "/"
System.out.println(new String(bytesURL)); // c3ViamVjdHM_X2Q=   notice the "_"

Base64.Decoder dec = Base64.getDecoder();
Base64.Decoder decURL = Base64.getUrlDecoder();

byte[] decodedURL = decURL.decode(bytesURL);
byte[] decoded = dec.decode(bytes);

System.out.println(new String(decodedURL));
System.out.println(new String(decoded));
```

