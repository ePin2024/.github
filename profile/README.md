# 사내 개발 가이드

## 📋 목차
- [개요](#개요)
- [개발 환경 설정](#개발-환경-설정)
- [프로젝트 구조](#프로젝트-구조)
- [개발 워크플로우](#개발-워크플로우)
- [코딩 컨벤션](#코딩-컨벤션)
- [테스트 가이드라인](#테스트-가이드라인)
- [배포 프로세스](#배포-프로세스)
- [문제 해결](#문제-해결)
- [기여 방법](#기여-방법)

## 개요

이 문서는 사내 개발팀을 위한 공용 개발 가이드입니다. 모든 개발자들이 일관된 방식으로 개발할 수 있도록 돕고, 효율적인 협업을 지원하기 위해 작성되었습니다.

### 주요 기술 스택
- **언어**: Kotlin, JavaScript
- **프레임워크**: Spring Boot, React.js
- **데이터베이스**: PostgreSQL, MySQL
- **도구**: IntelliJ IDEA, Cursor AI

## 개발 환경 설정

### 사전 요구사항
- **백엔드 개발**:
  - JDK 17 이상
  - IntelliJ IDEA
  - Gradle
- **프론트엔드 개발**:
  - Node.js (버전 18 이상)
  - npm 또는 yarn
- **공통**:
  - Git
  - Docker (선택사항)

### 로컬 개발 환경 구성

#### 백엔드 (Spring Boot) 설정
```bash
# 저장소 클론
git clone [프로젝트-저장소-URL]
cd [프로젝트명]

# Gradle 빌드
./gradlew build

# 애플리케이션 실행
./gradlew bootRun
```

#### 프론트엔드 (React.js) 설정
```bash
# 프론트엔드 디렉토리로 이동
cd frontend

# 의존성 설치
npm install

# 개발 서버 실행
npm start
```

### IDE 설정
#### 백엔드 개발 (IntelliJ IDEA)
- **필수 플러그인**:
  - Kotlin
  - Spring Boot
  - Database Navigator
  - Git Toolbox

#### 프론트엔드 개발 (Cursor AI 또는 VSCode)
- **필수 확장**:
  - ES7+ React/Redux/React-Native snippets
  - Prettier - Code formatter
  - ESLint
  - Auto Rename Tag

## 프로젝트 구조

```
프로젝트명/
├── backend/                # 백엔드 (Spring Boot - DDD 구조)
│   ├── src/main/kotlin/
│   │   ├── domain/        # 도메인 계층
│   │   │   ├── model/     # 도메인 모델 (Entity, Value Object)
│   │   │   ├── service/   # 도메인 서비스
│   │   │   └── repository/ # 도메인 리포지토리 인터페이스
│   │   ├── application/   # 애플리케이션 계층
│   │   │   ├── service/   # 애플리케이션 서비스
│   │   │   ├── usecase/   # 유스케이스
│   │   │   └── dto/       # 데이터 전송 객체
│   │   ├── infrastructure/ # 인프라스트럭처 계층
│   │   │   ├── persistence/ # 데이터베이스 구현
│   │   │   ├── external/  # 외부 시스템 연동
│   │   │   └── config/    # 인프라 설정
│   │   ├── presentation/  # 프레젠테이션 계층
│   │   │   ├── controller/ # REST 컨트롤러
│   │   │   └── dto/       # API 요청/응답 DTO
│   │   └── shared/        # 공통 계층
│   │       ├── util/      # 유틸리티
│   │       └── exception/ # 예외 처리
│   ├── src/main/resources/ # 설정 파일
│   └── build.gradle.kts   # Gradle 설정
├── frontend/              # 프론트엔드 (React.js)
│   ├── src/
│   │   ├── components/    # 재사용 가능한 컴포넌트
│   │   ├── pages/         # 페이지 컴포넌트
│   │   ├── hooks/         # 커스텀 훅
│   │   ├── utils/         # 유틸리티 함수
│   │   └── styles/        # 스타일 파일
│   ├── public/            # 정적 자산
│   └── package.json       # 프로젝트 설정
├── docs/                  # 문서
└── README.md             # 프로젝트 개요
```

## 개발 워크플로우

### 브랜치 전략
- `main`: 프로덕션 브랜치
- `develop`: 개발 브랜치
- `feature/[기능명]`: 기능 개발 브랜치
- `bugfix/[버그명]`: 버그 수정 브랜치

### 개발 프로세스

1. **이슈 생성**: 작업할 내용에 대한 이슈를 생성합니다
2. **브랜치 생성**: 적절한 브랜치를 생성합니다
   ```bash
   git checkout -b feature/새로운-기능
   ```
3. **개발**: 코드를 작성하고 테스트합니다
4. **커밋**: [커밋 메시지 컨벤션](#커밋-메시지-컨벤션)을 따라 커밋합니다
5. **Pull Request**: 코드 리뷰를 위한 PR을 생성합니다

### 커밋 메시지 컨벤션

```
타입(범위): 간단한 설명

자세한 설명 (선택사항)

Fixes #이슈번호
```

**타입**:
- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 변경
- `style`: 코드 포맷팅
- `refactor`: 리팩토링
- `test`: 테스트 추가/수정
- `chore`: 기타 변경사항

## 코딩 컨벤션

### 일반 규칙
#### 백엔드 (Kotlin)
- 들여쓰기는 4칸 공백 사용 (Kotlin 공식 컨벤션)
- 세미콜론 생략 권장 (Kotlin에서는 선택사항)
- 변수명은 camelCase 사용
- 상수는 UPPER_SNAKE_CASE 사용

#### 프론트엔드 (JavaScript/React)
- 들여쓰기는 2칸 공백 사용
- 세미콜론 사용 권장 (ESLint 설정에 따라)
- 변수명은 camelCase 사용
- 상수는 UPPER_SNAKE_CASE 사용

### 파일 및 폴더 명명 규칙
- 컴포넌트 파일: PascalCase (예: `UserProfile.jsx`)
- 유틸리티 파일: camelCase (예: `dateUtils.js`)
- 폴더명: kebab-case (예: `user-management`)

### 코드 스타일
#### 백엔드 (Kotlin)
- ktlint 설정을 따라주세요
- 코드 포맷팅: `./gradlew ktlintFormat`
- 린트 체크: `./gradlew ktlintCheck`

#### 프론트엔드 (JavaScript/React)
- ESLint와 Prettier 설정을 따라주세요
- 코드 포맷팅: `npm run format`
- 린트 체크: `npm run lint`

## 백엔드 상세 코딩 가이드

### Kotlin 코딩 스타일

#### 명명 규칙
```kotlin
// 클래스: PascalCase
class UserService
class OrderRepository

// 함수/변수: camelCase
fun getUserById(id: Long): User?
val userName = "john_doe"

// 상수: UPPER_SNAKE_CASE
const val MAX_RETRY_COUNT = 3
const val DEFAULT_PAGE_SIZE = 20

// 패키지: 소문자, 점으로 구분
package com.company.user.domain.model
```

#### 함수 작성 가이드
```kotlin
// ✅ 좋은 예: 명확한 함수명과 타입 지정
fun findUserByEmail(email: String): User? {
    return userRepository.findByEmail(email)
}

// ✅ 함수 인자가 많을 때는 여러 줄로
fun createUser(
    name: String,
    email: String,
    phoneNumber: String,
    address: String
): User {
    // 구현
}

// ✅ 확장 함수 활용
fun String.validEmailFlag(): Boolean {
    return this.contains("@") && this.contains(".")
}
```

#### Null Safety 활용
```kotlin
// ✅ Safe Call 연산자 사용
val userName = user?.name ?: "Unknown"

// ✅ let 함수 활용
user?.let { 
    sendNotification(it.email)
    logUserActivity(it.id)
}

// ✅ 조건부 실행
if (user != null) {
    processUser(user)
}
```

### DDD 구현 가이드

#### Entity 설계 주요 원칙

**val vs var 선택**:
- **var**: 변경 가능한 속성 (대부분의 Entity 필드)
- **val**: 불변 속성 (ID, createdAt 등 생성 후 변경되지 않는 필드)

**data class vs class**:
- **class 권장**: Entity는 비즈니스 로직을 포함하므로 일반 class 사용
- **data class 지양**: equals/hashCode가 모든 필드를 기준으로 생성되어 문제 발생 가능

**JPA 기본 생성자**:
- JPA는 리플렉션을 통해 기본 생성자로 객체를 생성
- Kotlin에서는 명시적으로 기본 생성자 제공 필요

#### 1. Domain Model (엔티티/값 객체)
```kotlin
// Entity 예제
@Entity
@Table(name = "users")
class User(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long = 0,
    
    @Column(nullable = false)
    var name: String,
    
    @Column(nullable = false, unique = true)
    var email: Email,
    
    @Enumerated(EnumType.STRING)
    var status: UserStatus = UserStatus.ACTIVE,
    
    @CreationTimestamp
    @Column(name = "created_at", nullable = false, updatable = false)
    val createdAt: LocalDateTime = LocalDateTime.now(),
    
    @UpdateTimestamp
    @Column(name = "updated_at", nullable = false)
    var updatedAt: LocalDateTime = LocalDateTime.now()
) {
    // JPA 기본 생성자
    constructor() : this(0, "", Email(""), UserStatus.ACTIVE)
    
    fun activate() {
        status = UserStatus.ACTIVE
        updatedAt = LocalDateTime.now()
    }
    
    fun deactivate() {
        status = UserStatus.INACTIVE
        updatedAt = LocalDateTime.now()
    }
    
    fun updateName(newName: String) {
        require(newName.isNotBlank()) { "Name cannot be blank" }
        name = newName
        updatedAt = LocalDateTime.now()
    }
    
    fun flagActive(): Boolean = status == UserStatus.ACTIVE
    
    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (other !is User) return false
        return id == other.id
    }
    
    override fun hashCode(): Int = id.hashCode()
    
    override fun toString(): String = "User(id=$id, name='$name', email=$email, status=$status)"
}

// Value Object 예제
@Embeddable
data class Email(
    @Column(name = "email")
    private val value: String
) {
    init {
        require(validFlag(value)) { "Invalid email format: $value" }
    }
    
    private fun validFlag(email: String): Boolean {
        return email.matches(Regex("^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$"))
    }
    
    override fun toString(): String = value
}

enum class UserStatus {
    ACTIVE, INACTIVE, SUSPENDED
}
```

#### 2. Domain Service
```kotlin
@Component
class UserDomainService(
    private val userRepository: UserRepository,
    private val emailValidator: EmailValidator
) {
    
    fun validateUserRegistration(user: User): ValidationResult {
        val errors = mutableListOf<String>()
        
        // 이메일 중복 체크
        if (userRepository.existsByEmail(user.email)) {
            errors.add("Email already exists")
        }
        
        // 도메인 규칙 검증
        if (!emailValidator.validFlag(user.email.toString())) {
            errors.add("Invalid email format")
        }
        
        return if (errors.isEmpty()) {
            ValidationResult.success()
        } else {
            ValidationResult.failure(errors)
        }
    }
}
```

#### 3. Repository Interface (Domain)
```kotlin
interface UserRepository {
    fun save(user: User): User
    fun findById(id: Long): User?
    fun findByEmail(email: Email): User?
    fun existsByEmail(email: Email): Boolean
    fun findAll(pageable: Pageable): Page<User>
    fun delete(user: User)
}
```

#### 4. Repository Implementation (Infrastructure)
```kotlin
@Repository
class JpaUserRepository(
    private val jpaRepository: UserJpaRepository
) : UserRepository {
    
    override fun save(user: User): User {
        return jpaRepository.save(user)
    }
    
    override fun findById(id: Long): User? {
        return jpaRepository.findById(id).orElse(null)
    }
    
    override fun findByEmail(email: Email): User? {
        return jpaRepository.findByEmail(email.toString())
    }
    
    override fun existsByEmail(email: Email): Boolean {
        return jpaRepository.existsByEmail(email.toString())
    }
    
    override fun findAll(pageable: Pageable): Page<User> {
        return jpaRepository.findAll(pageable)
    }
    
    override fun delete(user: User) {
        jpaRepository.delete(user)
    }
}

@Repository
interface UserJpaRepository : JpaRepository<User, Long> {
    fun findByEmail(email: String): User?
    fun existsByEmail(email: String): Boolean
}
```

#### 5. Application Service
```kotlin
@Service
@Transactional(readOnly = true)
class UserApplicationService(
    private val userRepository: UserRepository,
    private val userDomainService: UserDomainService,
    private val eventPublisher: ApplicationEventPublisher
) {
    
    @Transactional
    fun createUser(command: CreateUserCommand): UserResponse {
        val user = User(
            name = command.name,
            email = Email(command.email)
        )
        
        // 도메인 검증
        val validationResult = userDomainService.validateUserRegistration(user)
        if (!validationResult.successFlag) {
            throw ValidationException(validationResult.errors)
        }
        
        // 저장
        val savedUser = userRepository.save(user)
        
        // 이벤트 발행
        eventPublisher.publishEvent(UserCreatedEvent(savedUser.id, savedUser.email))
        
        return UserResponse.from(savedUser)
    }
    
    fun getUserById(id: Long): UserResponse {
        val user = userRepository.findById(id)
            ?: throw EntityNotFoundException("User not found with id: $id")
        
        return UserResponse.from(user)
    }
    
    fun getUsersByPage(page: Int, size: Int): Page<UserResponse> {
        val pageable = PageRequest.of(page, size, Sort.by("createdAt").descending())
        return userRepository.findAll(pageable).map { UserResponse.from(it) }
    }
}
```

#### 6. DTO 및 Command/Query 객체
```kotlin
// Command 객체
data class CreateUserCommand(
    val name: String,
    val email: String
) {
    init {
        require(name.isNotBlank()) { "Name cannot be blank" }
        require(email.isNotBlank()) { "Email cannot be blank" }
    }
}

// Response DTO
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String,
    val status: String,
    val createdAt: LocalDateTime
) {
    companion object {
        fun from(user: User): UserResponse {
            return UserResponse(
                id = user.id,
                name = user.name,
                email = user.email.toString(),
                status = user.status.name,
                createdAt = user.createdAt
            )
        }
    }
}
```

### REST API 설계 가이드

#### Controller 구현
```kotlin
@RestController
@RequestMapping("/api/v1/users")
@Validated
class UserController(
    private val userApplicationService: UserApplicationService
) {
    
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    fun createUser(
        @Valid @RequestBody request: CreateUserRequest
    ): ResponseEntity<ApiResponse<UserResponse>> {
        val command = CreateUserCommand(
            name = request.name,
            email = request.email
        )
        
        val user = userApplicationService.createUser(command)
        
        return ResponseEntity.ok(
            ApiResponse.success(
                data = user,
                message = "User created successfully"
            )
        )
    }
    
    @GetMapping("/{id}")
    fun getUserById(
        @PathVariable @Min(1) id: Long
    ): ResponseEntity<ApiResponse<UserResponse>> {
        val user = userApplicationService.getUserById(id)
        
        return ResponseEntity.ok(
            ApiResponse.success(data = user)
        )
    }
    
    @GetMapping
    fun getUsers(
        @RequestParam(defaultValue = "0") @Min(0) page: Int,
        @RequestParam(defaultValue = "20") @Min(1) @Max(100) size: Int
    ): ResponseEntity<ApiResponse<Page<UserResponse>>> {
        val users = userApplicationService.getUsersByPage(page, size)
        
        return ResponseEntity.ok(
            ApiResponse.success(data = users)
        )
    }
}

// API 요청 DTO
data class CreateUserRequest(
    @field:NotBlank(message = "Name is required")
    @field:Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    val name: String,
    
    @field:NotBlank(message = "Email is required")
    @field:Email(message = "Invalid email format")
    val email: String
)
```

#### 표준 API 응답 형식
```kotlin
data class ApiResponse<T>(
    val success: Boolean,
    val data: T?,
    val message: String?,
    val errors: List<String>?,
    val timestamp: LocalDateTime = LocalDateTime.now()
) {
    companion object {
        fun <T> success(data: T, message: String? = null): ApiResponse<T> {
            return ApiResponse(
                success = true,
                data = data,
                message = message,
                errors = null
            )
        }
        
        fun <T> failure(message: String, errors: List<String>? = null): ApiResponse<T> {
            return ApiResponse(
                success = false,
                data = null,
                message = message,
                errors = errors
            )
        }
    }
}
```

### 예외 처리 가이드

#### 커스텀 예외 정의
```kotlin
// 기본 비즈니스 예외
abstract class BusinessException(
    message: String,
    val errorCode: String,
    cause: Throwable? = null
) : RuntimeException(message, cause)

// 구체적인 예외들
class EntityNotFoundException(message: String) : 
    BusinessException(message, "ENTITY_NOT_FOUND")

class ValidationException(val errors: List<String>) : 
    BusinessException("Validation failed", "VALIDATION_ERROR")

class DuplicateEntityException(message: String) : 
    BusinessException(message, "DUPLICATE_ENTITY")
```

#### 글로벌 예외 처리
```kotlin
@ControllerAdvice
class GlobalExceptionHandler {
    
    private val logger = LoggerFactory.getLogger(GlobalExceptionHandler::class.java)
    
    @ExceptionHandler(EntityNotFoundException::class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    fun handleEntityNotFoundException(ex: EntityNotFoundException): ApiResponse<Nothing> {
        logger.warn("Entity not found: ${ex.message}")
        return ApiResponse.failure(ex.message ?: "Entity not found")
    }
    
    @ExceptionHandler(ValidationException::class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    fun handleValidationException(ex: ValidationException): ApiResponse<Nothing> {
        logger.warn("Validation error: ${ex.errors}")
        return ApiResponse.failure("Validation failed", ex.errors)
    }
    
    @ExceptionHandler(MethodArgumentNotValidException::class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    fun handleMethodArgumentNotValid(ex: MethodArgumentNotValidException): ApiResponse<Nothing> {
        val errors = ex.bindingResult.fieldErrors.map { "${it.field}: ${it.defaultMessage}" }
        logger.warn("Validation error: $errors")
        return ApiResponse.failure("Validation failed", errors)
    }
    
    @ExceptionHandler(Exception::class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    fun handleGenericException(ex: Exception): ApiResponse<Nothing> {
        logger.error("Unexpected error occurred", ex)
        return ApiResponse.failure("An unexpected error occurred")
    }
}
```

### 데이터베이스 및 JPA 가이드

#### JPA Index 설정 가이드

**@Table의 indexes 속성**
```kotlin
@Table(
    name = "orders",
    indexes = [
        Index(name = "idx_user_id", columnList = "user_id"),
        Index(name = "idx_created_at", columnList = "created_at"),
        Index(name = "idx_status_created", columnList = "status, created_at")
    ]
)
```

**Index 설정 목적**:
- **조회 성능 향상**: WHERE 절에서 자주 사용되는 컬럼에 인덱스 생성
- **정렬 성능 향상**: ORDER BY 절에서 사용되는 컬럼에 인덱스 생성
- **조인 성능 향상**: Foreign Key 컬럼에 인덱스 생성

**Index 명명 규칙**:
- 단일 컬럼: `idx_컬럼명`
- 복합 컬럼: `idx_컬럼1_컬럼2`
- 유니크 인덱스: `uk_컬럼명`

**주의사항**:
- 인덱스는 SELECT 성능을 향상시키지만 INSERT/UPDATE/DELETE 성능을 저하시킴
- 필요한 인덱스만 생성 (과도한 인덱스는 오히려 성능 저하)
- 복합 인덱스는 컬럼 순서가 중요 (선택도가 높은 컬럼을 앞에)

#### Entity 매핑 베스트 프랙티스
```kotlin
@Entity
@Table(
    name = "orders",
    indexes = [
        Index(name = "idx_user_id", columnList = "user_id"),
        Index(name = "idx_created_at", columnList = "created_at")
    ]
)
class Order(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    val user: User,
    
    @OneToMany(
        mappedBy = "order", 
        cascade = [CascadeType.ALL], 
        orphanRemoval = true,
        fetch = FetchType.LAZY
    )
    val orderItems: MutableList<OrderItem> = mutableListOf(),
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    val status: OrderStatus = OrderStatus.PENDING,
    
    @Column(name = "total_amount", nullable = false, precision = 10, scale = 2)
    val totalAmount: BigDecimal,
    
    @CreationTimestamp
    @Column(name = "created_at", nullable = false, updatable = false)
    val createdAt: LocalDateTime = LocalDateTime.now(),
    
    @UpdateTimestamp
    @Column(name = "updated_at", nullable = false)
    val updatedAt: LocalDateTime = LocalDateTime.now()
) {
    fun addOrderItem(orderItem: OrderItem) {
        orderItems.add(orderItem)
        orderItem.order = this
    }
    
    fun removeOrderItem(orderItem: OrderItem) {
        orderItems.remove(orderItem)
        orderItem.order = null
    }
}
```

#### 복잡한 쿼리 처리
```kotlin
@Repository
interface OrderRepository : JpaRepository<Order, Long> {
    
    @Query("""
        SELECT o FROM Order o
        JOIN FETCH o.user u
        WHERE o.status = :status
        AND o.createdAt BETWEEN :startDate AND :endDate
        ORDER BY o.createdAt DESC
    """)
    fun findOrdersByStatusAndDateRange(
        status: OrderStatus,
        startDate: LocalDateTime,
        endDate: LocalDateTime,
        pageable: Pageable
    ): Page<Order>
    
    @Query("""
        SELECT new com.company.dto.OrderSummary(
            o.id, 
            u.name, 
            o.totalAmount, 
            o.status, 
            o.createdAt
        )
        FROM Order o
        JOIN o.user u
        WHERE u.id = :userId
        ORDER BY o.createdAt DESC
    """)
    fun findOrderSummariesByUserId(
        userId: Long,
        pageable: Pageable
    ): Page<OrderSummary>
}
```

### 테스트 작성 가이드

#### 단위 테스트
```kotlin
@ExtendWith(MockitoExtension::class)
class UserApplicationServiceTest {
    
    @Mock
    private lateinit var userRepository: UserRepository
    
    @Mock
    private lateinit var userDomainService: UserDomainService
    
    @Mock
    private lateinit var eventPublisher: ApplicationEventPublisher
    
    @InjectMocks
    private lateinit var userApplicationService: UserApplicationService
    
    @Test
    fun `should create user successfully`() {
        // Given
        val command = CreateUserCommand("John Doe", "john@example.com")
        val user = User(name = command.name, email = Email(command.email))
        val savedUser = user.copy(id = 1L)
        
        whenever(userDomainService.validateUserRegistration(any()))
            .thenReturn(ValidationResult.success())
        whenever(userRepository.save(any<User>()))
            .thenReturn(savedUser)
        
        // When
        val result = userApplicationService.createUser(command)
        
        // Then
        assertThat(result.id).isEqualTo(1L)
        assertThat(result.name).isEqualTo("John Doe")
        assertThat(result.email).isEqualTo("john@example.com")
        
        verify(userRepository).save(any<User>())
        verify(eventPublisher).publishEvent(any<UserCreatedEvent>())
    }
    
    @Test
    fun `should throw exception when user not found`() {
        // Given
        val userId = 999L
        whenever(userRepository.findById(userId)).thenReturn(null)
        
        // When & Then
        assertThatThrownBy { 
            userApplicationService.getUserById(userId) 
        }
        .isInstanceOf(EntityNotFoundException::class.java)
        .hasMessage("User not found with id: $userId")
    }
}
```

#### 통합 테스트
```kotlin
@SpringBootTest
@TestPropertySource(locations = ["classpath:application-test.properties"])
@Transactional
class UserIntegrationTest {
    
    @Autowired
    private lateinit var userApplicationService: UserApplicationService
    
    @Autowired
    private lateinit var userRepository: UserRepository
    
    @Test
    fun `should create and retrieve user`() {
        // Given
        val command = CreateUserCommand("Integration Test", "integration@test.com")
        
        // When
        val createdUser = userApplicationService.createUser(command)
        val retrievedUser = userApplicationService.getUserById(createdUser.id)
        
        // Then
        assertThat(retrievedUser.name).isEqualTo("Integration Test")
        assertThat(retrievedUser.email).isEqualTo("integration@test.com")
        assertThat(retrievedUser.status).isEqualTo("ACTIVE")
    }
}
```

### 성능 최적화 가이드

#### N+1 문제 해결
```kotlin
// ❌ N+1 문제 발생
fun getBadOrders(): List<OrderResponse> {
    return orderRepository.findAll().map { order ->
        OrderResponse(
            id = order.id,
            userName = order.user.name, // 지연 로딩으로 N+1 발생
            itemCount = order.orderItems.size // 지연 로딩으로 N+1 발생
        )
    }
}

// ✅ Fetch Join으로 해결
@Query("""
    SELECT DISTINCT o FROM Order o
    JOIN FETCH o.user
    JOIN FETCH o.orderItems
""")
fun findAllWithUserAndItems(): List<Order>

// ✅ EntityGraph 사용
@EntityGraph(attributePaths = ["user", "orderItems"])
fun findAllWithGraph(): List<Order>
```

#### 캐싱 활용
```kotlin
@Service
class UserApplicationService {
    
    @Cacheable(value = ["users"], key = "#id")
    fun getUserById(id: Long): UserResponse {
        // 구현
    }
    
    @CacheEvict(value = ["users"], key = "#result.id")
    fun updateUser(command: UpdateUserCommand): UserResponse {
        // 구현
    }
    
    @CacheEvict(value = ["users"], allEntries = true)
    fun clearUserCache() {
        // 캐시 전체 삭제
    }
}
```

## 테스트 가이드라인

### 테스트 종류
- **단위 테스트**: 개별 함수/컴포넌트 테스트
- **통합 테스트**: 여러 모듈 간의 상호작용 테스트
- **E2E 테스트**: 전체 애플리케이션 플로우 테스트

### 테스트 실행

#### 백엔드 테스트
```bash
# 모든 테스트 실행
./gradlew test

# 특정 테스트 클래스 실행
./gradlew test --tests "*UserServiceTest"

# 테스트 커버리지 확인
./gradlew jacocoTestReport
```

#### 프론트엔드 테스트
```bash
# 모든 테스트 실행
npm test

# 특정 테스트 파일 실행
npm test -- --testNamePattern="UserProfile"

# 테스트 커버리지 확인
npm run test:coverage
```

### 테스트 작성 규칙
- 테스트 파일은 `[파일명].test.js` 형식으로 명명
- 각 기능에 대해 최소 80% 이상의 코드 커버리지 유지
- 테스트는 독립적이고 반복 가능해야 함

## 배포 프로세스

### 개발 환경 배포
```bash
npm run deploy:dev
```

### 프로덕션 배포
1. `develop` 브랜치를 `main`으로 병합
2. 태그 생성: `git tag v1.0.0`
3. 자동 배포 트리거 또는 수동 배포 실행

### 환경별 설정
- **개발**: `config/dev.json`
- **스테이징**: `config/staging.json`
- **프로덕션**: `config/prod.json`

## 문제 해결

### 자주 발생하는 문제들

#### 의존성 설치 문제
```bash
# node_modules 삭제 후 재설치
rm -rf node_modules package-lock.json
npm install
```

#### 포트 충돌 문제
```bash
# 포트 사용 프로세스 확인
lsof -i :3000
# 프로세스 종료
kill -9 [PID]
```

### 로그 확인
- 개발 서버 로그: 터미널에서 확인
- 프로덕션 로그: `logs/` 디렉토리 확인

## 기여 방법

### 이슈 리포팅
1. 프로젝트의 GitHub Issues에서 새 이슈 생성
2. 버그 리포트 또는 기능 요청 템플릿 사용
3. 재현 가능한 예제 코드 포함

### 코드 기여
1. 저장소 포크
2. 새 브랜치 생성
3. 코드 작성 및 테스트
4. Pull Request 생성
5. 코드 리뷰 대응

### 문서 기여
- 오타 수정, 내용 개선, 새로운 가이드 추가 등
- Markdown 형식을 따라 작성

## 연락처 및 지원

- **개발팀 리더**: [팀장 이름] (이메일)
- **사내 개발자 채널**: [Slack/Teams 채널명]
- **기술 문의**: [기술지원 채널 또는 이메일]

---

마지막 업데이트: 2024년 1월

이 문서는 지속적으로 업데이트됩니다. 질문이나 제안사항이 있으시면 언제든 이슈를 생성해주세요!
