> 이 포스트는 [널널한 개발자](https://www.inflearn.com/course/%EA%B3%B0%EC%B1%85-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/dashboard)님의 강의를 듣고 작성한 글입니다.

## 컴퓨터가 3층집인건 알죠?

운영체제는 S/W다. 일종의 platform이다. 가상화기술은 H/W를 S/W로 구현한 것이 가상화 기술이다.

운영체제는 MS Word와 같은 S/W다. 단, 차이는 user application 단의 process들을 support한다. 즉, application process들이 잘 작동하도록 도와주는 것이다. 또 하나 다른 점은 H/W를 제어 및 관리한다. 대부분 범용 OS가 멀티 태스킹 즉, 멀티 프로세싱 시스템이다. 그리고 나쁜 프로세스들을 감지하여 나쁜짓을 하지 못하게 막는 역할도 한다.

### H/W 용어 정리

- Interrupt: 한마디로 '벨 소리'이다. 내가 CPU라고 가정하고 나를 방해하는 요소가 있는데 그 요소가 Interrupt 즉, 예로 컴퓨터와 주변기기와 I/O 하는데 그 때마다 interrupt 발행한다. interrupt가 발생하면 computer가 잠깐 멈추고 wait하다가 다시 interrupt가 오면 진행된다.

### printf 함수 호출하는 과정

1. process 내에 App에서 printf 함수 호출
2. API 내부에서는 장치라는 것을 추상화한 file이라는 인터페이스를 통해서 정보가 내려간다.
3. 정보가 내려가면 환경이 kernel 영역으로 바뀌는데 이 때 어느 새로운 코드가 실행이 된다. 이 때 이 진입점의 있는 코드를 System call이라고 한다. 즉, printf는 System call의 write명령이고 이것이 작동되며 구성요소가 작동된다.
4. 이 후에 Device Driver를 제어하기 시작한다.
5. Device Driver가 제어가 되면 interrupt를 요청한다. 이때 나오는 말이 Interrupt Request (IRQ)이다.
6. IRQ는 고유번호를 가지는데 그 번호를 가지고 요청하기 되면 Interrupt가 발생하면서 CPU는 잠시 멈추고 해당 장치와 통신을 하게 된다.
7. 해당장치와 연결 후 수행할 명령을 외부장치에 실행한다. 예로 모니터에 hello라는 글자를 렌더링한다.
8. 할 일을 끝내면 Device Driver는 Interrupt를 보내면서 할 일 다했다고 위로 요청을 한다. (kernel 구성요소에게)
9. API에서 process로 리턴이된다.

결론적으로 이 프로세스가 1~9까지의 일련의 과정이 끝나서 흐름이 넘어올 때까지 wait를 하면 이 모든 I/O는 blocking I/O가 되는거고 wait하지 않고 다른 일을 하면 non-blocking I/O가 된다.
