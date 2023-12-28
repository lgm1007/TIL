# Jest와 Mocking을 통한 유닛 테스트
### Jest
* 코드가 동작하는 지 테스트 케이스를 통해 확인하기 위한 테스트 프레임워크
* Nestjs에 기본으로 탑재되어 있다.
* test 패키지나 Controller, Gateway를 만들면 자동으로 `.spec.ts` 파일이 생성되는데, 해당 파일이 테스트 코드를 작성하기 위한 파일이다.
    * `app.controller.spec.ts` 파일 내용
```ts
import { Test, TestingModule } from '@nestjs/testing';
import { AppController } from './app.controller';
import { AppService } from './app.service';

describe('AppController', () => {
    let appController: AppController;
    
    beforeEach(async () => {
        const app: TestingModule = await Test.createTestingModule({
            controllers: [AppController],
            providers: [AppService],
        }).compile();
        appController = app.get<AppController>(AppController);
    });
    
    describe('root', () => {
        it('should return "Hello World!"', () => {
            expect(appController.getHello()).toBe('Hello World!');
        });
    });
});
```
#### Jest 기본 문법
* `describe()`
  * 테스트 단위를 묶어주며, 묶은 단위에 대한 설명을 작성할 수 있다.
  * `describe()` 안에 또 `describe()`를 작성할 수도 있다.
* `it()` 또는 `test()`
  * `describe()`로 묶은 단위에 테스트를 실행하는 함수
* `beforeEach()`와 `afterEach()`
  * 각각의 테스트 케이스를 실행하기 전, 후에 작동하는 코드
  * 예를 들어 테스트 케이스가 3개가 있다면, `beforeEach()`와 `afterEach()`는 각각 3번씩 실행될 것이다.
* `beforeAll()`와 `afterAll()`
  * 해당 파일에서 테스트가 시작하기 전에 한 번, 모든 테스트가 끝난 후에 한 번 실행되는 함수
  * 테스트 케이스마다 실행이 필요한 게 아니라면 해당 함수를 사용하는 게 효율적이다.
* `expect()`
  * 테스트 케이스의 실행 결과가 원하는 값과 일치하는 지 확인하는 함수

### Mocking
* 함수의 실행 결과값을 내가 지정한 값으로 대체하는 작업을 의미한다.
* 예를 들어 Mocking을 사용하면 Service 레이어에서 데이터베이스에 연결하지 않고 개발자가 지정한 값으로 테스트를 진행하게 되어 독립적인 테스트 케이스를 작성할 수 있다.

#### Mocking으로 테스트 환경 준비
```ts
const mockUserRepository = {
    save: jest.fn(),
    findOne: jest.fn(),
};

/*
MockRepository를 type alias로 정의,
Record로 Repository<T>의 메서드를 추출한 값을 key로, jest.Mock을 value 값으로 갖는 타입을 리턴
Repository의 메서드 전부를 사용할 것은 아니기 때문에 Partial으로 메서드를 선택적으로 가져옴
 */
type MockRepository<T = any> = Partial<Record<keyof Repository<T>, jest.Mock>>;

describe('UserService', () => {
    let service: UserService;
    let userRepository: MockRepository<User>;
    
    beforeEach(async () => {
        const module: TestingModule = await Test.createTestingModule({
            providers: [
                UserService,
                { provide: getRepositoryToken(User), useValue: mockUserRepository },
            ],
        }).compile();
        
        service = module.get<UserService>(UserService);
        userRepository = module.get<MockRepository<User>>(getRepositoryToken(User));
    });

    it('should be defined', () => {
        expert(service).toBeDefined();
    });
});
```
* `MockRepository`로 `Repository<T>`를 Mocking한다.
* 예제에서 Mocking할 Repository는 하나였기 때문에 `mockUserRepository`를 object 형태로 정의했지만, 여러 Repository를 Mocking할 때는 함수 형태로 작성하는 게 좋다.
  * ```ts
    const mockUserRepository = () => ({
        save: jest.fn(),
        findOne: jest.fn(),
    });
    ```
* `mockUserRepository`에서 사용할 메서드는 `save`와 `findOne`이기 때문에 두 메서드를 `jest.fn()`으로 메서드를 Mocking 해준다.
* `getRepositoryToken`을 custom provider로 사용했는데, `@InjectRepository()`를 사용하여 `UserRepository`를 요청할 때마다 `useValue`로 등록된 `mockUserRepository` 객체를 사용하게 된다.
  * `getRepositoryToken`은 typeORM에서 제공하는 메서드
* 위와 같이 설정해서 테스트 코드를 작동할 때마다 데이터베이스에 직접 접속하게 되는 것이 아닌 Mocking한 Repository를 호출하게 된다.

#### 유닛 테스트 작성하기
```ts
describe('createUser', () => {
    const createUserArgument = {
        email: 'test@example.com',
        password: '12345',
        name: 'test1',
        phoneNumber: '010-1234-5678',
    };
    
    it('should create user', async () => {
        userRepository.findOne.mockResolvedValue(undefined);
        userRepository.save.mockResolvedValue(createUserArgument);
        
        const result = await service.createUser(createUserArgument);
        
        expect(userRepository.save).toHaveBeenCalledTimes(1);
        expect(result).toEqual({ success: true });
    });
});
```
* `createUserArgument`: 유저 생성을 위한 임의의 유저 데이터
* `mockResolvedValue`: promise를 반환할 때 사용하는 메서드.
  * `save`는 `Promise<User>`를 반환하기 때문에 인자로 `createUserArgument`를 설정했다.
* `toHaveBeenCalledTimes()`: 해당 메서드가 몇 번 호출되었는지 체크

#### Mocking 설정하기 예제 2
```ts
// Repository를 Mocking하기 위한 fake repo
// 테스트하고자 하는 VerifyService 내 Repository 에서 사용하는 메서드가 findOne, save, create가 있다고 한다.
// fake repo가 다른 typeORM의 Entity들이 공유해서는 안 되기 때문에 함수 형태로 작성한다.
const mockRepository = () => ({
  findOne: jest.fn(),
  save: jest.fn(),
  create: jest.fn(),
});

// service Mocking을 위한 fake service
// fake service는 다른 곳에서 중복되어 사용하지 않으니 객체 형태로 만들어도 된다.
const mockJwtService = {
  sign: jest.fn(), 
  verify: jest.fn(),
};

const mockMailService = {
  sendEmail: jest.fn(),
};

type MockRepository<T = any> = Partial<Record<keyof Repository<T>, jest.Mock>>;

// VerifyService 테스트
describe('VerifyService', () => {
    let service: VerifyService;
    let verificationRepository: MockRepository<Verification>;

    beforeAll(async() => {
        const modules: TestingModule = await Test.createTestingModule({
          provider: [
            VerifyService,
            { provide: getRepositoryToken(Verification), useValue: mockRepository() },   // mocking Verification Repo
            { provide: getRepositoryToken(User), useValue: mockRepository() },          // mocking User Repo
            { provide: JwtService, useValue: mockJwtService },                          // mocking Service
            { provide: MailService, useValue: mockMailService },
          ],
        }).compile();

        service = modules.get<VerifyService>(VerifyService);
        
        verificationRepository = modules.get(getRepositoryToken(Verification));     // 사용할 mocking Repo
    });

    it('be defined', () => {
        expect(service).toBeDefined();
    });
});
```

