# RXMemoEx
RXMemoEx내용정리
뷰모델은 뷰컨트롤러의 속성으로 추가한다.
뷰모델의 속성과 뷰를 바인딩한다.

Observable(옵저버블) 시퀀스을 "스트림"이라고 표현한다.
메모를 편집할때 사용하는 뷰모델

Driver는 error메시지를 전달하지 않는 obsevable이다.
observable에서share연산자를 share(replay:1, scope: .whileCommanded) 하는것과 동일하게 동작한다.
모든 구독자가 시퀀스를 공유하고, 새로운 구독자는 구독이후에 방출되는 이벤트만 전달 받는다. 즉 가장최근에 전달된 이벤트가 즉시 전달된다.
observable에 새로운 구독자가 추가되면, 새로운 시퀀스가 시작된다.
즉 새로운 시퀀스가 시작된다말은 observable에 새로운 구독자가 추가된것이다.
시퀀스수가 binding수가 된다.
observable은 이벤트를 통해서 요소를 방출한다.
share연산자를 추가하면,모든 구독자가 하나의 시퀀스를 구독공유한다.
binder는 error이벤트를 받지 않는다,즉 error이벤트는 binding하지 못한다.
Driver는 모든작업이 MainThread에서 실행되는것을 보장하고,스퀀스를 공유하기 때문에 share연산자는 불필요하다,
Driver는 스퀀스를 공유하기 때문에 share 연산자를 사용하지 않는다.
보통은 Driver를 직접생성하지 않는다,asDriver를 사용해서 일반 observable을 driver로 변환한다.

앱을 구성하고 있는 모든신은 네비게이션 컨트롤러에 인베디드 되기때문에,네비게이션 타이틀이 필요하다.
드라이버 형식으로 타이틀 속성을 추가하면,네비게이션 아이템을 쉽게 바인딩할수있다.
viewModel.title.drive(navigationitem.rx.title)

class CommonViewModel: NSObject {
 /*
 뷰모델의 속성과 뷰를 drive로 바인딩한다.viewModel.title의 값이 변견될때 마다 navigationItem의 title도 자동으로 업데이트된다.viewModel.title가 더이상 사용되지 않을때, rx.disposeBag에 의해서 메모리를 해제한다고 설정
 viewModel.title.drive(navigationItem,rx.title).disposed(by: rx.disposeBag)
 */
 //드라이버는 시퀀스를 공유한다. 뷰모델의 속성과 뷰를 drive로 바인딩한다.
 let title: Driver<String>
 //실제 형식이 아닌 프로토콜로 선언하면,한가지 타입에 대한 의존성을 쉽게 해결할수있다.
 let sceneCoordinator: SceneCoordinatorType
 let storage: MemoStorage

 //속성 초기화 생성자 구현
 init(title: String, sceneCoordinator: SceneCoordinatorType, storage: MemoStorageType){
 //Driver.just로 observable을 생성한후 asDriver로 driver를 변환해서 반환한다.
 self.title = Driver.just(title).asDriver(onErrorJustReturn: "")

 self.sceneCoordinator = sceneCoordinator
 self.storage = storage
 }
 
}



이벤트는 메인스케줄러로 전달된다.
class MemoComposeViewModel: CommonViewModel
