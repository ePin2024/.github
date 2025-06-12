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

#### RESTful Endpoint 설계 원칙

**1. 리소스 중심 설계**
```bash
# ✅ 좋은 예: 명사형 리소스 사용
GET    /api/v1/users          # 사용자 목록 조회
POST   /api/v1/users          # 사용자 생성
GET    /api/v1/users/{id}     # 특정 사용자 조회
PUT    /api/v1/users/{id}     # 사용자 전체 수정
PATCH  /api/v1/users/{id}     # 사용자 부분 수정
DELETE /api/v1/users/{id}     # 사용자 삭제

# ❌ 나쁜 예: 동사형 사용
POST /api/v1/createUser       # 동사 사용 금지
GET  /api/v1/getUser/{id}     # 동사 사용 금지
POST /api/v1/deleteUser/{id}  # 잘못된 HTTP 메서드
```

**2. HTTP 메서드별 사용 규칙**

| HTTP 메서드 | 목적 | 멱등성 | 요청 Body | 응답 Body |
|-------------|------|--------|-----------|-----------|
| **GET** | 조회 | ✅ | ❌ | ✅ |
| **POST** | 생성 | ❌ | ✅ | ✅ |
| **PUT** | 전체 수정 | ✅ | ✅ | ✅ |
| **PATCH** | 부분 수정 | ❌ | ✅ | ✅ |
| **DELETE** | 삭제 | ✅ | ❌ | ✅ |

**3. URL 패턴 및 명명 규칙**
```bash
# 기본 패턴: /api/{version}/{resource}
/api/v1/users
/api/v1/orders
/api/v1/products

# 계층적 리소스: 부모/자식 관계
GET    /api/v1/users/{userId}/orders           # 특정 사용자의 주문 목록
POST   /api/v1/users/{userId}/orders           # 특정 사용자의 주문 생성
GET    /api/v1/users/{userId}/orders/{orderId} # 특정 사용자의 특정 주문
PUT    /api/v1/orders/{orderId}/items/{itemId} # 주문의 특정 아이템 수정

# 복합 리소스 조회
GET /api/v1/orders/{orderId}/items              # 주문의 아이템 목록
GET /api/v1/categories/{categoryId}/products    # 카테고리별 상품 목록
```

**4. 쿼리 파라미터 활용**
```bash
# 필터링
GET /api/v1/users?status=active&role=admin
GET /api/v1/orders?status=pending&userId=123

# 정렬
GET /api/v1/users?sort=createdAt,desc
GET /api/v1/products?sort=price,asc&sort=name,desc

# 페이징
GET /api/v1/users?page=0&size=20
GET /api/v1/orders?page=1&size=50&sort=createdAt,desc

# 검색
GET /api/v1/users?search=john
GET /api/v1/products?name=contains:phone&price=gte:100000

# 필드 선택 (부분 응답)
GET /api/v1/users?fields=id,name,email
GET /api/v1/orders?fields=id,status,totalAmount
```

**5. HTTP 상태 코드 가이드**

**성공 응답 (2xx)**
```kotlin
// 200 OK: 조회/수정 성공
@GetMapping("/{id}")
@ResponseStatus(HttpStatus.OK)
fun getUser(@PathVariable id: Long): ApiResponse<UserResponse>

// 201 Created: 생성 성공
@PostMapping
@ResponseStatus(HttpStatus.CREATED)
fun createUser(@RequestBody request: CreateUserRequest): ApiResponse<UserResponse>

// 204 No Content: 삭제 성공 (응답 Body 없음)
@DeleteMapping("/{id}")
@ResponseStatus(HttpStatus.NO_CONTENT)
fun deleteUser(@PathVariable id: Long)
```

**클라이언트 오류 (4xx)**
```kotlin
// 400 Bad Request: 잘못된 요청
return ResponseEntity.badRequest().body(
    ApiResponse.failure("Invalid request parameters")
)

// 401 Unauthorized: 인증 필요
return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body(
    ApiResponse.failure("Authentication required")
)

// 403 Forbidden: 권한 없음
return ResponseEntity.status(HttpStatus.FORBIDDEN).body(
    ApiResponse.failure("Access denied")
)

// 404 Not Found: 리소스 없음
return ResponseEntity.notFound().build()

// 409 Conflict: 리소스 충돌
return ResponseEntity.status(HttpStatus.CONFLICT).body(
    ApiResponse.failure("Email already exists")
)

// 422 Unprocessable Entity: 검증 실패
return ResponseEntity.unprocessableEntity().body(
    ApiResponse.failure("Validation failed", errors)
)
```

**6. API 버저닝 전략**
```bash
# URL 버저닝 (권장)
/api/v1/users
/api/v2/users

# Header 버저닝
GET /api/users
Accept: application/vnd.company.v1+json

# 파라미터 버저닝
GET /api/users?version=1
```

**7. 실제 Controller 구현 예제**
```kotlin
@RestController
@RequestMapping("/api/v1/users")
@Validated
class UserController(
    private val userApplicationService: UserApplicationService
) {
    
    // 사용자 목록 조회 (페이징, 필터링, 정렬)
    @GetMapping
    fun getUsers(
        @RequestParam(defaultValue = "0") @Min(0) page: Int,
        @RequestParam(defaultValue = "20") @Min(1) @Max(100) size: Int,
        @RequestParam(defaultValue = "createdAt,desc") sort: String,
        @RequestParam(required = false) status: UserStatus?,
        @RequestParam(required = false) search: String?
    ): ResponseEntity<ApiResponse<Page<UserResponse>>> {
        
        val pageable = PageRequest.of(page, size, parseSort(sort))
        val users = userApplicationService.getUsers(pageable, status, search)
        
        return ResponseEntity.ok(ApiResponse.success(users))
    }
    
    // 특정 사용자 조회
    @GetMapping("/{id}")
    fun getUserById(
        @PathVariable @Min(1) id: Long
    ): ResponseEntity<ApiResponse<UserResponse>> {
        val user = userApplicationService.getUserById(id)
        return ResponseEntity.ok(ApiResponse.success(user))
    }
    
    // 사용자 생성
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
        
        return ResponseEntity.status(HttpStatus.CREATED).body(
            ApiResponse.success(
                data = user,
                message = "User created successfully"
            )
        )
    }
    
    // 사용자 전체 수정
    @PutMapping("/{id}")
    fun updateUser(
        @PathVariable @Min(1) id: Long,
        @Valid @RequestBody request: UpdateUserRequest
    ): ResponseEntity<ApiResponse<UserResponse>> {
        val command = UpdateUserCommand(
            id = id,
            name = request.name,
            email = request.email,
            status = request.status
        )
        
        val user = userApplicationService.updateUser(command)
        
        return ResponseEntity.ok(
            ApiResponse.success(
                data = user,
                message = "User updated successfully"
            )
        )
    }
    
    // 사용자 부분 수정
    @PatchMapping("/{id}")
    fun patchUser(
        @PathVariable @Min(1) id: Long,
        @Valid @RequestBody request: PatchUserRequest
    ): ResponseEntity<ApiResponse<UserResponse>> {
        val command = PatchUserCommand(
            id = id,
            name = request.name,
            status = request.status
        )
        
        val user = userApplicationService.patchUser(command)
        
        return ResponseEntity.ok(
            ApiResponse.success(
                data = user,
                message = "User updated successfully"
            )
        )
    }
    
    // 사용자 삭제
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    fun deleteUser(
        @PathVariable @Min(1) id: Long
    ): ResponseEntity<Void> {
        userApplicationService.deleteUser(id)
        return ResponseEntity.noContent().build()
    }
    
    // 사용자의 주문 목록 조회 (계층적 리소스)
    @GetMapping("/{userId}/orders")
    fun getUserOrders(
        @PathVariable @Min(1) userId: Long,
        @RequestParam(defaultValue = "0") @Min(0) page: Int,
        @RequestParam(defaultValue = "20") @Min(1) @Max(100) size: Int
    ): ResponseEntity<ApiResponse<Page<OrderResponse>>> {
        val pageable = PageRequest.of(page, size)
        val orders = orderApplicationService.getOrdersByUserId(userId, pageable)
        
        return ResponseEntity.ok(ApiResponse.success(orders))
    }
    
    // 정렬 파라미터 파싱 유틸리티
    private fun parseSort(sortParam: String): Sort {
        return try {
            val parts = sortParam.split(",")
            val property = parts[0]
            val direction = if (parts.size > 1 && parts[1].lowercase() == "desc") {
                Sort.Direction.DESC
            } else {
                Sort.Direction.ASC
            }
            Sort.by(direction, property)
        } catch (e: Exception) {
            Sort.by(Sort.Direction.DESC, "createdAt")  // 기본값
        }
    }
}
```

**8. 요청/응답 DTO 설계**
```kotlin
// 생성 요청 DTO
data class CreateUserRequest(
    @field:NotBlank(message = "Name is required")
    @field:Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    val name: String,
    
    @field:NotBlank(message = "Email is required")
    @field:Email(message = "Invalid email format")
    val email: String
)

// 전체 수정 요청 DTO (모든 필드 필수)
data class UpdateUserRequest(
    @field:NotBlank(message = "Name is required")
    @field:Size(min = 2, max = 50)
    val name: String,
    
    @field:NotBlank(message = "Email is required")
    @field:Email(message = "Invalid email format")
    val email: String,
    
    @field:NotNull(message = "Status is required")
    val status: UserStatus
)

// 부분 수정 요청 DTO (모든 필드 선택사항)
data class PatchUserRequest(
    @field:Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    val name: String? = null,
    
    val status: UserStatus? = null
)

// 응답 DTO
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String,
    val status: String,
    val createdAt: LocalDateTime,
    val updatedAt: LocalDateTime
) {
    companion object {
        fun from(user: User): UserResponse {
            return UserResponse(
                id = user.id,
                name = user.name,
                email = user.email.toString(),
                status = user.status.name,
                createdAt = user.createdAt,
                updatedAt = user.updatedAt
            )
        }
    }
}
```

**9. API 문서화 (OpenAPI/Swagger)**
```kotlin
@RestController
@RequestMapping("/api/v1/users")
@Tag(name = "User", description = "사용자 관리 API")
class UserController {
    
    @Operation(
        summary = "사용자 목록 조회",
        description = "페이징, 필터링, 정렬이 가능한 사용자 목록을 조회합니다."
    )
    @ApiResponses(value = [
        ApiResponse(responseCode = "200", description = "조회 성공"),
        ApiResponse(responseCode = "400", description = "잘못된 요청 파라미터")
    ])
    @GetMapping
    fun getUsers(
        @Parameter(description = "페이지 번호 (0부터 시작)", example = "0")
        @RequestParam(defaultValue = "0") page: Int,
        
        @Parameter(description = "페이지 크기", example = "20")
        @RequestParam(defaultValue = "20") size: Int
    ): ResponseEntity<ApiResponse<Page<UserResponse>>> {
        // 구현
    }
}
```

#### Spring Validation 어노테이션 설명

// ... existing code ...

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
