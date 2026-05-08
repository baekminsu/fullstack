# 입출력, 파일 입출력, 동시성

입출력은 프로그램 밖의 세계와 데이터를 주고받는 일입니다. 파일, 네트워크, 콘솔, DB(Database) 접근은 모두 입출력 비용과 실패 가능성을 가집니다.

## InputStream과 OutputStream

바이트 단위 입출력입니다.

```java
try (InputStream in = Files.newInputStream(Path.of("input.txt"));
     OutputStream out = Files.newOutputStream(Path.of("copy.txt"))) {
    byte[] buffer = new byte[1024];
    int length;
    while ((length = in.read(buffer)) != -1) {
        out.write(buffer, 0, length);
    }
}
```

`try-with-resources`를 사용하면 자원을 자동으로 닫습니다.

## Reader와 Writer

문자 단위 입출력입니다.

```java
try (BufferedReader reader = Files.newBufferedReader(Path.of("input.txt"));
     BufferedWriter writer = Files.newBufferedWriter(Path.of("output.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        writer.write(line);
        writer.newLine();
    }
}
```

문자 인코딩이 중요할 때는 `StandardCharsets.UTF_8`을 명시합니다.

```java
Files.readString(Path.of("data.txt"), StandardCharsets.UTF_8);
```

## Files API(Application Programming Interface)

NIO(New Input Output)의 `Files`는 파일 작업을 간결하게 처리합니다.

```java
Path path = Path.of("memo.txt");
Files.writeString(path, "hello", StandardCharsets.UTF_8);
String content = Files.readString(path, StandardCharsets.UTF_8);
```

목록 읽기:

```java
try (Stream<Path> paths = Files.list(Path.of("."))) {
    paths.forEach(System.out::println);
}
```

파일 존재 확인:

```java
if (Files.exists(path)) {
    System.out.println("exists");
}
```

## 파일 업로드와 서버 보안

Spring에서 파일 업로드를 받을 때는 아래를 조심합니다.

- 원본 파일명을 그대로 저장하지 않습니다.
- 확장자만 믿지 않습니다.
- 저장 경로가 외부 입력으로 조작되지 않게 합니다.
- 파일 크기 제한을 둡니다.
- 정적 리소스로 바로 노출할지, 인증 후 다운로드할지 결정합니다.

나쁜 예:

```java
Path target = Path.of(uploadDir, multipartFile.getOriginalFilename());
```

더 나은 방향:

```java
String storedName = UUID.randomUUID() + ".bin";
Path target = Path.of(uploadDir).resolve(storedName).normalize();
```

## 직렬화

직렬화는 객체를 저장하거나 전송 가능한 형태로 바꾸는 일입니다. Java 기본 직렬화는 보안과 호환성 문제가 있어 서버 API에서는 JSON(JavaScript Object Notation)을 주로 사용합니다.

Spring Boot는 Jackson을 이용해 Java 객체와 JSON을 변환합니다.

```java
public record UserResponse(Long id, String email) {
}
```

응답:

```json
{
  "id": 1,
  "email": "a@example.com"
}
```

## 스레드

스레드는 하나의 프로세스 안에서 실행되는 작업 흐름입니다.

```java
Thread thread = new Thread(() -> System.out.println("work"));
thread.start();
```

Spring MVC(Model View Controller)는 일반적으로 요청마다 서버 스레드가 배정되어 컨트롤러, 서비스, 리포지토리를 실행합니다.

## ExecutorService

스레드를 직접 만들기보다 스레드 풀을 사용합니다.

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
executor.submit(() -> {
    System.out.println("background work");
});
executor.shutdown();
```

스레드 풀 크기를 무작정 키우면 CPU, 메모리, DB 커넥션 풀이 함께 압박을 받습니다.

## synchronized

동시에 접근하면 안 되는 영역을 보호합니다.

```java
public synchronized void increase() {
    count++;
}
```

단일 JVM 안에서는 동작하지만, 서버가 여러 대로 늘어나면 DB 잠금, Redis, 메시지 큐 같은 외부 조정 장치가 필요할 수 있습니다.

## 동시성 실무 질문

- 동시에 같은 쿠폰을 발급하면 중복 발급되는가?
- 동시에 재고를 차감하면 음수가 되는가?
- 스레드 풀은 DB 커넥션 풀보다 훨씬 큰가?
- 타임아웃 없는 외부 API 호출이 요청 스레드를 오래 점유하는가?
- 파일 업로드 중 실패하면 임시 파일을 정리하는가?

## 체크리스트

- [ ] try-with-resources로 입출력 자원을 닫는다.
- [ ] 문자 파일은 인코딩을 명시한다.
- [ ] 파일 업로드에서 원본 파일명과 경로 조작을 조심한다.
- [ ] 스레드 풀과 DB 커넥션 풀의 관계를 설명할 수 있다.
- [ ] 동시성 문제를 메모리, DB, 분산 환경으로 나누어 생각한다.
