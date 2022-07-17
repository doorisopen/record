# story9. IO에서 발생하는 병목 현상
생성일: 2022년 6월 21일 오후 10:22

---

IO는 성능에 영향을 가장 많이 미친다.

IO에서 발생하는 시간은 CPU 대기시간에 속한다.

### 바이트 기반의 스트림 입력

- ByteArrayInputStream: 바이트로 구성된 배열을 읽어서 입력 스트림을 만든다.
- FileInputStream: 이미지와 같은 바이너리 기반의 파일의 스트림을 만든다.
- FilterInputStream: 여러 종류의 유용한 입력 스트림의 추상 클래스이다.
- ObjectInputStream: ObjectOutputStream을 통해서 저장해 놓은 객체를 읽기 위한 스트림을 만든다.
- PipedInputStream: PipedOutputStream을 통해서 출력된 스트림을 읽어서 처리하기 위한 스트림을 만든다.
- SequenceInputStream: 별개인 두 개의 스트림을 하나의 스트림으로 만든다.

### 문자열 기반의 스트림

java.io.Reader

- BufferedReader: 문자열 입력 스트림을 버퍼에 담아서 처리한다. 일반적으로 문자열 기반의 파일을 읽을 때 가장 많이 사용된다.
- CharArrayReader: char의 배열로 된 문자 배열을 처리한다.
- FilterReader: 문자열 기반의 스트림을 처리하기 위한 추상 클래스이다.
- FileReader: 문자열 기반의 파일을 읽기 위한 클래스이다.
- InputStreamReader: 바이트 기반의 스트림을 문자열 기반의 스트림으로 연결하는 역할을 수행한다.
- PipedReader: 파이프 스트림을 읽는다.
- StringReader: 문자열 기반의 소스를 읽는다.

한번 수행한 스트림은 반드시 **자원 해제**를 해야한다. 그렇지 않으면 리소스가 부족해 질 수 있다.

일반적으로 **문자열 단위로 파일을 읽는 경우 BufferedReader 클래스**를 사용합니다.

```java
public static ArrayList<String> readBufferedReader(String fileName) throws Exception {
		ArrayList<String> list = new ArrayList<>();
		BufferedReader br = null;
		try {
				br = new BufferedReader(new FileReader(fileName));
				String data;
				while ((data = br.readLine()) != null) {
						list.addElement(data);
				}
		} catch (Exception e) {
				System.err.println(e.getMessage());
				throw e;
		} finally {
				if (br != null) br.close();
		}
		return list;
}
```

# NIO

- 시스템 IO 관련 문제가 있으면 NIO를 학습한다.
- 내용이 아주 방대해서 따로 학습하는 것이 좋다.
- JDK 7 이상에 NIO2 에 추가된 기능이 있다.

# Path, Watch

- JDK 6 추가
- 파일 모니터링 클래스