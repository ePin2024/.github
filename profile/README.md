# 에잇핀 개발 가이드

## 📋 목차
- [개요](#개요)
- [개발 환경 설정](#개발-환경-설정)
- [프로젝트 구조](#프로젝트-구조)
- [개발 워크플로우](#개발-워크플로우)
- [코딩 컨벤션](#코딩-컨벤션)
- [백엔드 상세 코딩 가이드](#백엔드-상세-코딩-가이드)
- [프론트엔드 상세 코딩 가이드](#프론트엔드-상세-코딩-가이드)
- [테스트 가이드라인](#테스트-가이드라인)
- [배포 프로세스](#배포-프로세스)
- [문제 해결](#문제-해결)
- [기여 방법](#기여-방법)

## 개요

이 문서는 사내 개발팀을 위한 공용 개발 가이드입니다. 모든 개발자들이 일관된 방식으로 개발할 수 있도록 돕고, 효율적인 협업을 지원하기 위해 작성되었습니다.

### 주요 기술 스택
- **언어**: Kotlin, JavaScript
- **프레임워크**: Spring Boot, React.js
- **데이터베이스**: PostgreSQL, MySQL, MongoDB, Redis
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

### 백엔드 코딩 컨벤션 (Kotlin)
- **들여쓰기**: 4칸 공백 사용 (Kotlin 공식 컨벤션)
- **세미콜론**: 생략 권장 (Kotlin에서는 선택사항)
- **변수명**: camelCase 사용
- **상수**: UPPER_SNAKE_CASE 사용
- **클래스명**: PascalCase 사용
- **패키지명**: 소문자, 점으로 구분
- **Boolean 함수**: `flag` 접두사 사용 (예: `flagActive()`)

### 프론트엔드 코딩 컨벤션 (JavaScript/React)
- **들여쓰기**: 2칸 공백 사용
- **세미콜론**: 사용 권장 (ESLint 설정에 따라)
- **변수명**: camelCase 사용
- **상수**: UPPER_SNAKE_CASE 사용
- **컴포넌트명**: PascalCase 사용
- **파일명**: PascalCase (컴포넌트), camelCase (유틸리티)
- **폴더명**: kebab-case 사용

### 공통 명명 규칙
- **컴포넌트 파일**: PascalCase (예: `UserProfile.jsx`)
- **유틸리티 파일**: camelCase (예: `dateUtils.js`)
- **폴더명**: kebab-case (예: `user-management`)
- **API 엔드포인트**: kebab-case (예: `/api/v1/user-profiles`)

### 코드 스타일 도구
#### 백엔드 (Kotlin)
- **Linter**: ktlint
- **코드 포맷팅**: `./gradlew ktlintFormat`
- **린트 체크**: `./gradlew ktlintCheck`

#### 프론트엔드 (JavaScript/React)
- **Linter**: ESLint + Prettier
- **코드 포맷팅**: `npm run format`
- **린트 체크**: `npm run lint`

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

### Redis 설정 및 사용 가이드

#### 1. Redis 설정

**application.yml 설정**
```yaml
spring:
  redis:
    host: ${REDIS_HOST:localhost}
    port: ${REDIS_PORT:6379}
    password: ${REDIS_PASSWORD:}
    timeout: 2000ms
    lettuce:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
        max-wait: -1ms
    database: 0  # 기본 데이터베이스 인덱스
```

**RedisConfig 설정 클래스**
```kotlin
@Configuration
@EnableRedisRepositories
class RedisConfig {
    
    @Value("\${spring.redis.host}")
    private lateinit var host: String
    
    @Value("\${spring.redis.port}")
    private var port: Int = 6379
    
    @Value("\${spring.redis.password}")
    private lateinit var password: String
    
    @Bean
    fun redisConnectionFactory(): LettuceConnectionFactory {
        val configuration = RedisStandaloneConfiguration(host, port)
        if (password.isNotBlank()) {
            configuration.password = RedisPassword.of(password)
        }
        return LettuceConnectionFactory(configuration)
    }
    
    @Bean
    fun redisTemplate(): RedisTemplate<String, Any> {
        val template = RedisTemplate<String, Any>()
        template.connectionFactory = redisConnectionFactory()
        
        // JSON 직렬화 설정
        val jackson2JsonRedisSerializer = Jackson2JsonRedisSerializer(Any::class.java)
        val objectMapper = ObjectMapper()
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY)
        objectMapper.activateDefaultTyping(
            LaissezFaireSubTypeValidator.instance,
            ObjectMapper.DefaultTyping.NON_FINAL
        )
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper)
        
        // Key-Value 직렬화 설정
        template.keySerializer = StringRedisSerializer()
        template.valueSerializer = jackson2JsonRedisSerializer
        template.hashKeySerializer = StringRedisSerializer()
        template.hashValueSerializer = jackson2JsonRedisSerializer
        
        template.afterPropertiesSet()
        return template
    }
    
    @Bean
    fun stringRedisTemplate(): StringRedisTemplate {
        return StringRedisTemplate(redisConnectionFactory())
    }
}
```

#### 2. Redis 사용 패턴

**캐시 서비스 구현**
```kotlin
@Service
class RedisCacheService(
    private val redisTemplate: RedisTemplate<String, Any>,
    private val stringRedisTemplate: StringRedisTemplate
) {
    
    companion object {
        private const val USER_CACHE_PREFIX = "user:"
        private const val SESSION_PREFIX = "session:"
        private const val CACHE_TTL = 3600L // 1시간
    }
    
    // 객체 캐시 저장
    fun cacheUser(userId: Long, user: User, ttlSeconds: Long = CACHE_TTL) {
        val key = "$USER_CACHE_PREFIX$userId"
        redisTemplate.opsForValue().set(key, user, Duration.ofSeconds(ttlSeconds))
    }
    
    // 객체 캐시 조회
    fun getCachedUser(userId: Long): User? {
        val key = "$USER_CACHE_PREFIX$userId"
        return redisTemplate.opsForValue().get(key) as? User
    }
    
    // 캐시 삭제
    fun evictUserCache(userId: Long) {
        val key = "$USER_CACHE_PREFIX$userId"
        redisTemplate.delete(key)
    }
    
    // 패턴 기반 캐시 삭제
    fun evictUserCacheByPattern(pattern: String = "$USER_CACHE_PREFIX*") {
        val keys = redisTemplate.keys(pattern)
        if (keys.isNotEmpty()) {
            redisTemplate.delete(keys)
        }
    }
    
    // 세션 관리
    fun createSession(sessionId: String, userId: Long, ttlSeconds: Long = 1800L) {
        val key = "$SESSION_PREFIX$sessionId"
        val sessionData = mapOf(
            "userId" to userId,
            "createdAt" to System.currentTimeMillis(),
            "lastAccessTime" to System.currentTimeMillis()
        )
        redisTemplate.opsForHash<String, Any>().putAll(key, sessionData)
        redisTemplate.expire(key, Duration.ofSeconds(ttlSeconds))
    }
    
    // 세션 조회
    fun getSession(sessionId: String): Map<String, Any>? {
        val key = "$SESSION_PREFIX$sessionId"
        val sessionData = redisTemplate.opsForHash<String, Any>().entries(key)
        
        return if (sessionData.isNotEmpty()) {
            // 마지막 접근 시간 업데이트
            redisTemplate.opsForHash<String, Any>().put(key, "lastAccessTime", System.currentTimeMillis())
            sessionData
        } else {
            null
        }
    }
    
    // 카운터 증가 (조회수, 좋아요 등)
    fun incrementCounter(key: String): Long {
        return stringRedisTemplate.opsForValue().increment(key) ?: 0L
    }
    
    // 카운터 값 조회
    fun getCounter(key: String): Long {
        return stringRedisTemplate.opsForValue().get(key)?.toLongOrNull() ?: 0L
    }
    
    // Rate Limiting
    fun flagRateLimited(key: String, limit: Int, windowSeconds: Long): Boolean {
        val count = stringRedisTemplate.opsForValue().increment(key) ?: 0L
        
        if (count == 1L) {
            stringRedisTemplate.expire(key, Duration.ofSeconds(windowSeconds))
        }
        
        return count > limit
    }
}
```

**애플리케이션 서비스에서 캐시 활용**
```kotlin
@Service
@Transactional(readOnly = true)
class UserApplicationService(
    private val userRepository: UserRepository,
    private val redisCacheService: RedisCacheService
) {
    
    fun getUserById(id: Long): UserResponse {
        // 1. 캐시에서 먼저 조회
        redisCacheService.getCachedUser(id)?.let { cachedUser ->
            return UserResponse.from(cachedUser)
        }
        
        // 2. 캐시에 없으면 데이터베이스에서 조회
        val user = userRepository.findById(id)
            ?: throw EntityNotFoundException("User not found with id: $id")
        
        // 3. 조회한 데이터를 캐시에 저장
        redisCacheService.cacheUser(id, user)
        
        return UserResponse.from(user)
    }
    
    @Transactional
    fun updateUser(id: Long, request: UpdateUserRequest): UserResponse {
        val user = userRepository.findById(id)
            ?: throw EntityNotFoundException("User not found with id: $id")
        
        user.updateName(request.name)
        val updatedUser = userRepository.save(user)
        
        // 캐시 업데이트
        redisCacheService.cacheUser(id, updatedUser)
        
        return UserResponse.from(updatedUser)
    }
    
    @Transactional
    fun deleteUser(id: Long) {
        userRepository.findById(id)?.let { user ->
            userRepository.delete(user)
            // 캐시에서 삭제
            redisCacheService.evictUserCache(id)
        }
    }
}
```

#### 3. Spring Cache 어노테이션 활용

**CacheConfig 설정**
```kotlin
@Configuration
@EnableCaching
class CacheConfig {
    
    @Bean
    fun cacheManager(): CacheManager {
        val redisCacheManager = RedisCacheManager.Builder(redisConnectionFactory())
            .cacheDefaults(
                RedisCacheConfiguration.defaultCacheConfig()
                    .entryTtl(Duration.ofHours(1))
                    .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(StringRedisSerializer()))
                    .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(Jackson2JsonRedisSerializer(Any::class.java)))
            )
            .build()
        
        return redisCacheManager
    }
}
```

**어노테이션 기반 캐싱**
```kotlin
@Service
class UserService(
    private val userRepository: UserRepository
) {
    
    @Cacheable(value = ["users"], key = "#id")
    fun getUserById(id: Long): User? {
        return userRepository.findById(id)
    }
    
    @Cacheable(value = ["users"], key = "#email.value")
    fun getUserByEmail(email: Email): User? {
        return userRepository.findByEmail(email)
    }
    
    @CacheEvict(value = ["users"], key = "#id")
    fun evictUserCache(id: Long) {
        // 캐시만 삭제
    }
    
    @CacheEvict(value = ["users"], allEntries = true)
    fun evictAllUserCache() {
        // 모든 사용자 캐시 삭제
    }
    
    @CachePut(value = ["users"], key = "#result.id")
    fun updateUser(id: Long, request: UpdateUserRequest): User {
        val user = userRepository.findById(id)
            ?: throw EntityNotFoundException("User not found")
        
        user.updateName(request.name)
        return userRepository.save(user)
    }
}
```

#### 4. 고급 Redis 사용 패턴

**분산 락 구현**
```kotlin
@Component
class RedisDistributedLock(
    private val stringRedisTemplate: StringRedisTemplate
) {
    
    fun acquireLock(lockKey: String, lockValue: String, expireTime: Long): Boolean {
        return stringRedisTemplate.opsForValue()
            .setIfAbsent(lockKey, lockValue, Duration.ofSeconds(expireTime)) ?: false
    }
    
    fun releaseLock(lockKey: String, lockValue: String): Boolean {
        val script = """
            if redis.call('get', KEYS[1]) == ARGV[1] then
                return redis.call('del', KEYS[1])
            else
                return 0
            end
        """.trimIndent()
        
        val result = stringRedisTemplate.execute(
            RedisScript.of(script, Long::class.java),
            listOf(lockKey),
            lockValue
        )
        
        return result == 1L
    }
}

// 사용 예시
@Service
class OrderService(
    private val distributedLock: RedisDistributedLock
) {
    
    fun processOrder(orderId: Long) {
        val lockKey = "order:lock:$orderId"
        val lockValue = UUID.randomUUID().toString()
        
        if (distributedLock.acquireLock(lockKey, lockValue, 30)) {
            try {
                // 주문 처리 로직
                processOrderInternal(orderId)
            } finally {
                distributedLock.releaseLock(lockKey, lockValue)
            }
        } else {
            throw IllegalStateException("Could not acquire lock for order: $orderId")
        }
    }
}
```

**Pub/Sub 메시징**
```kotlin
@Component
class RedisMessagePublisher(
    private val redisTemplate: RedisTemplate<String, Any>
) {
    
    fun publishMessage(channel: String, message: Any) {
        redisTemplate.convertAndSend(channel, message)
    }
}

@Component
class RedisMessageSubscriber : MessageListener {
    
    override fun onMessage(message: Message, pattern: ByteArray?) {
        val messageBody = String(message.body)
        val channel = String(message.channel)
        
        // 메시지 처리 로직
        processMessage(channel, messageBody)
    }
    
    private fun processMessage(channel: String, message: String) {
        when (channel) {
            "user.created" -> handleUserCreated(message)
            "user.updated" -> handleUserUpdated(message)
            else -> println("Unknown channel: $channel")
        }
    }
}

@Configuration
class RedisMessageConfig {
    
    @Bean
    fun redisMessageListenerContainer(
        connectionFactory: RedisConnectionFactory,
        messageSubscriber: RedisMessageSubscriber
    ): RedisMessageListenerContainer {
        val container = RedisMessageListenerContainer()
        container.setConnectionFactory(connectionFactory)
        container.addMessageListener(messageSubscriber, PatternTopic("user.*"))
        return container
    }
}
```

#### 5. 성능 최적화 및 모니터링

**Redis 성능 모니터링**
```kotlin
@Component
class RedisHealthIndicator(
    private val redisTemplate: RedisTemplate<String, Any>
) : HealthIndicator {
    
    override fun health(): Health {
        return try {
            val connection = redisTemplate.connectionFactory?.connection
            val info = connection?.info() ?: emptyMap()
            
            Health.up()
                .withDetail("redis", "Available")
                .withDetail("version", info["redis_version"])
                .withDetail("used_memory", info["used_memory_human"])
                .build()
        } catch (e: Exception) {
            Health.down()
                .withDetail("redis", "Not Available")
                .withException(e)
                .build()
        }
    }
}
```

**Redis 연결 풀 최적화**
```yaml
spring:
  redis:
    lettuce:
      pool:
        max-active: 16        # 최대 활성 연결 수
        max-idle: 8           # 최대 유휴 연결 수  
        min-idle: 2           # 최소 유휴 연결 수
        max-wait: 1000ms      # 연결 대기 시간
      shutdown-timeout: 100ms # 종료 대기 시간
    timeout: 3000ms          # 명령 실행 타임아웃
```

#### 6. Redis 개발/운영 팁

**개발 환경에서 Redis 사용**
```bash
# Docker로 Redis 실행
docker run -d --name redis -p 6379:6379 redis:7-alpine

# Redis CLI 접속
docker exec -it redis redis-cli

# 기본 명령어
redis-cli ping
redis-cli keys "*"
redis-cli flushall  # 모든 데이터 삭제 (개발용)
```

**성능 고려사항**
- **키 설계**: 네임스페이스 사용 (`user:123`, `session:abc`)
- **만료 시간**: 모든 캐시에 적절한 TTL 설정
- **메모리 관리**: maxmemory 정책 설정 (allkeys-lru 권장)
- **파이프라이닝**: 여러 명령어를 한 번에 실행하여 네트워크 왕복 최소화

**보안 고려사항**
- Redis 패스워드 설정
- 방화벽으로 포트 제한
- Redis Sentinel 또는 Cluster 사용 (고가용성)

### MongoDB 설정 및 사용 가이드

#### 1. MongoDB 설정

**build.gradle.kts 의존성 추가**
```kotlin
dependencies {
    implementation("org.springframework.boot:spring-boot-starter-data-mongodb")
    implementation("org.springframework.boot:spring-boot-starter-data-mongodb-reactive") // 반응형 사용 시
}
```

**application.yml 설정**
```yaml
spring:
  data:
    mongodb:
      uri: ${MONGODB_URI:mongodb://localhost:27017/appdb}
      # 또는 개별 설정
      host: ${MONGODB_HOST:localhost}
      port: ${MONGODB_PORT:27017}
      database: ${MONGODB_DATABASE:appdb}
      username: ${MONGODB_USERNAME:}
      password: ${MONGODB_PASSWORD:}
      authentication-database: admin
      
  # MongoDB 로깅 설정
logging:
  level:
    org.springframework.data.mongodb: DEBUG
    org.mongodb.driver: INFO
```

**MongoDB 설정 클래스**
```kotlin
@Configuration
@EnableMongoRepositories
class MongoConfig {
    
    @Value("\${spring.data.mongodb.uri}")
    private lateinit var mongoUri: String
    
    @Bean
    fun mongoClient(): MongoClient {
        val connectionString = ConnectionString(mongoUri)
        val settings = MongoClientSettings.builder()
            .applyConnectionString(connectionString)
            .codecRegistry(
                CodecRegistries.fromRegistries(
                    MongoClientSettings.getDefaultCodecRegistry(),
                    CodecRegistries.fromProviders(PojoCodecProvider.builder().automatic(true).build())
                )
            )
            .build()
        
        return MongoClients.create(settings)
    }
    
    @Bean
    fun mongoTemplate(mongoClient: MongoClient): MongoTemplate {
        return MongoTemplate(mongoClient, "appdb")
    }
    
    // 트랜잭션 매니저 설정 (MongoDB 4.0+ 복제본 세트에서만 지원)
    @Bean
    fun mongoTransactionManager(mongoDbFactory: MongoDatabaseFactory): MongoTransactionManager {
        return MongoTransactionManager(mongoDbFactory)
    }
}
```

#### 2. Document 모델링

**기본 Document 엔티티**
```kotlin
@Document(collection = "users")
data class UserDocument(
    @Id
    @Field("_id")
    val id: String? = null,
    
    @Field("name")
    val name: String,
    
    @Field("email")
    val email: String,
    
    @Field("age")
    val age: Int? = null,
    
    @Field("status")
    val status: UserStatus = UserStatus.ACTIVE,
    
    @Field("profile")
    val profile: UserProfile? = null,
    
    @Field("tags")
    val tags: List<String> = emptyList(),
    
    @Field("metadata")
    val metadata: Map<String, Any> = emptyMap(),
    
    @Field("created_at")
    val createdAt: LocalDateTime = LocalDateTime.now(),
    
    @Field("updated_at")
    val updatedAt: LocalDateTime = LocalDateTime.now()
) {
    // 비즈니스 로직 메서드
    fun flagActive(): Boolean = status == UserStatus.ACTIVE
    
    fun hasTag(tag: String): Boolean = tags.contains(tag)
    
    fun addTag(tag: String): UserDocument {
        return copy(tags = tags + tag, updatedAt = LocalDateTime.now())
    }
    
    fun updateProfile(newProfile: UserProfile): UserDocument {
        return copy(profile = newProfile, updatedAt = LocalDateTime.now())
    }
}

// 임베디드 도큐먼트
data class UserProfile(
    @Field("avatar_url")
    val avatarUrl: String? = null,
    
    @Field("bio")
    val bio: String? = null,
    
    @Field("social_links")
    val socialLinks: Map<String, String> = emptyMap(),
    
    @Field("preferences")
    val preferences: UserPreferences = UserPreferences()
)

data class UserPreferences(
    @Field("theme")
    val theme: String = "light",
    
    @Field("language")
    val language: String = "ko",
    
    @Field("notifications")
    val notifications: NotificationSettings = NotificationSettings()
)

data class NotificationSettings(
    @Field("email_enabled")
    val emailEnabled: Boolean = true,
    
    @Field("push_enabled")
    val pushEnabled: Boolean = true,
    
    @Field("marketing_enabled")
    val marketingEnabled: Boolean = false
)

enum class UserStatus {
    ACTIVE, INACTIVE, SUSPENDED, DELETED
}
```

**복잡한 Document 예제 (블로그 포스트)**
```kotlin
@Document(collection = "posts")
data class PostDocument(
    @Id
    val id: String? = null,
    
    @Field("title")
    val title: String,
    
    @Field("content")
    val content: String,
    
    @Field("author_id")
    val authorId: String,
    
    @Field("author_name")
    val authorName: String,
    
    @Field("category")
    val category: String,
    
    @Field("tags")
    val tags: List<String> = emptyList(),
    
    @Field("comments")
    val comments: List<Comment> = emptyList(),
    
    @Field("meta")
    val meta: PostMeta = PostMeta(),
    
    @Field("published")
    val published: Boolean = false,
    
    @Field("published_at")
    val publishedAt: LocalDateTime? = null,
    
    @Field("created_at")
    val createdAt: LocalDateTime = LocalDateTime.now(),
    
    @Field("updated_at")
    val updatedAt: LocalDateTime = LocalDateTime.now()
) {
    fun addComment(comment: Comment): PostDocument {
        return copy(
            comments = comments + comment,
            meta = meta.copy(commentCount = comments.size + 1),
            updatedAt = LocalDateTime.now()
        )
    }
    
    fun publish(): PostDocument {
        return copy(
            published = true,
            publishedAt = LocalDateTime.now(),
            updatedAt = LocalDateTime.now()
        )
    }
}

data class Comment(
    @Field("id")
    val id: String = ObjectId().toString(),
    
    @Field("author_id")
    val authorId: String,
    
    @Field("author_name")
    val authorName: String,
    
    @Field("content")
    val content: String,
    
    @Field("created_at")
    val createdAt: LocalDateTime = LocalDateTime.now()
)

data class PostMeta(
    @Field("view_count")
    val viewCount: Long = 0,
    
    @Field("like_count")
    val likeCount: Long = 0,
    
    @Field("comment_count")
    val commentCount: Long = 0,
    
    @Field("reading_time")
    val readingTime: Int = 0 // 분 단위
)
```

#### 3. Repository 패턴

**기본 Repository 인터페이스**
```kotlin
interface UserRepository : MongoRepository<UserDocument, String> {
    
    // 메서드 이름 기반 쿼리
    fun findByEmail(email: String): UserDocument?
    fun findByNameContainingIgnoreCase(name: String): List<UserDocument>
    fun findByStatus(status: UserStatus): List<UserDocument>
    fun findByTagsContaining(tag: String): List<UserDocument>
    fun findByCreatedAtBetween(start: LocalDateTime, end: LocalDateTime): List<UserDocument>
    
    // 페이징 지원
    fun findByStatus(status: UserStatus, pageable: Pageable): Page<UserDocument>
    
    // 복합 조건
    fun findByStatusAndCreatedAtAfter(status: UserStatus, createdAt: LocalDateTime): List<UserDocument>
    
    // 존재 여부 확인
    fun existsByEmail(email: String): Boolean
    
    // 카운트
    fun countByStatus(status: UserStatus): Long
    
    // 정렬
    fun findByStatusOrderByCreatedAtDesc(status: UserStatus): List<UserDocument>
}
```

**커스텀 Repository 구현**
```kotlin
interface UserRepositoryCustom {
    fun findUsersByComplexCriteria(criteria: UserSearchCriteria): List<UserDocument>
    fun findUsersWithAggregation(): List<UserStatsDto>
    fun updateUserTags(userId: String, tags: List<String>): UpdateResult
}

@Repository
class UserRepositoryCustomImpl(
    private val mongoTemplate: MongoTemplate
) : UserRepositoryCustom {
    
    override fun findUsersByComplexCriteria(criteria: UserSearchCriteria): List<UserDocument> {
        val query = Query()
        
        // 동적 쿼리 구성
        criteria.name?.let { 
            query.addCriteria(Criteria.where("name").regex(".*$it.*", "i"))
        }
        
        criteria.email?.let {
            query.addCriteria(Criteria.where("email").`is`(it))
        }
        
        criteria.status?.let {
            query.addCriteria(Criteria.where("status").`is`(it))
        }
        
        criteria.tags?.let { tags ->
            if (tags.isNotEmpty()) {
                query.addCriteria(Criteria.where("tags").`in`(tags))
            }
        }
        
        criteria.ageRange?.let { range ->
            query.addCriteria(Criteria.where("age").gte(range.min).lte(range.max))
        }
        
        criteria.createdAfter?.let {
            query.addCriteria(Criteria.where("created_at").gte(it))
        }
        
        // 정렬 및 페이징
        criteria.sortBy?.let { sortBy ->
            val sort = if (criteria.sortOrder == "desc") {
                Sort.by(Sort.Direction.DESC, sortBy)
            } else {
                Sort.by(Sort.Direction.ASC, sortBy)
            }
            query.with(sort)
        }
        
        criteria.limit?.let { limit ->
            query.limit(limit)
            criteria.offset?.let { offset ->
                query.skip(offset.toLong())
            }
        }
        
        return mongoTemplate.find(query, UserDocument::class.java)
    }
    
    override fun findUsersWithAggregation(): List<UserStatsDto> {
        val aggregation = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("status").`is`(UserStatus.ACTIVE)),
            Aggregation.group("status")
                .count().`as`("count")
                .avg("age").`as`("avgAge")
                .first("status").`as`("status"),
            Aggregation.sort(Sort.Direction.DESC, "count")
        )
        
        return mongoTemplate.aggregate(aggregation, "users", UserStatsDto::class.java).mappedResults
    }
    
    override fun updateUserTags(userId: String, tags: List<String>): UpdateResult {
        val query = Query(Criteria.where("id").`is`(userId))
        val update = Update()
            .set("tags", tags)
            .set("updated_at", LocalDateTime.now())
        
        return mongoTemplate.updateFirst(query, update, UserDocument::class.java)
    }
}

// 검색 조건 DTO
data class UserSearchCriteria(
    val name: String? = null,
    val email: String? = null,
    val status: UserStatus? = null,
    val tags: List<String>? = null,
    val ageRange: AgeRange? = null,
    val createdAfter: LocalDateTime? = null,
    val sortBy: String? = null,
    val sortOrder: String = "asc",
    val limit: Int? = null,
    val offset: Int? = null
)

data class AgeRange(val min: Int, val max: Int)
data class UserStatsDto(val status: UserStatus, val count: Long, val avgAge: Double)
```

#### 4. Service 계층 구현

**MongoDB 서비스 클래스**
```kotlin
@Service
@Transactional
class UserMongoService(
    private val userRepository: UserRepository,
    private val mongoTemplate: MongoTemplate
) {
    
    fun createUser(request: CreateUserRequest): UserDocument {
        // 이메일 중복 체크
        if (userRepository.existsByEmail(request.email)) {
            throw DuplicateEmailException("Email already exists: ${request.email}")
        }
        
        val userDocument = UserDocument(
            name = request.name,
            email = request.email,
            age = request.age,
            tags = request.tags ?: emptyList()
        )
        
        return userRepository.save(userDocument)
    }
    
    @Transactional(readOnly = true)
    fun getUserById(id: String): UserDocument {
        return userRepository.findById(id)
            .orElseThrow { EntityNotFoundException("User not found with id: $id") }
    }
    
    @Transactional(readOnly = true)
    fun getUserByEmail(email: String): UserDocument? {
        return userRepository.findByEmail(email)
    }
    
    @Transactional(readOnly = true)
    fun searchUsers(criteria: UserSearchCriteria): List<UserDocument> {
        return userRepository.findUsersByComplexCriteria(criteria)
    }
    
    @Transactional(readOnly = true)
    fun getUsersByStatus(status: UserStatus, pageable: Pageable): Page<UserDocument> {
        return userRepository.findByStatus(status, pageable)
    }
    
    fun updateUser(id: String, request: UpdateUserRequest): UserDocument {
        val existingUser = getUserById(id)
        
        val updatedUser = existingUser.copy(
            name = request.name ?: existingUser.name,
            age = request.age ?: existingUser.age,
            status = request.status ?: existingUser.status,
            updatedAt = LocalDateTime.now()
        )
        
        return userRepository.save(updatedUser)
    }
    
    fun addTagToUser(userId: String, tag: String): UserDocument {
        val user = getUserById(userId)
        val updatedUser = user.addTag(tag)
        return userRepository.save(updatedUser)
    }
    
    fun updateUserProfile(userId: String, profile: UserProfile): UserDocument {
        val user = getUserById(userId)
        val updatedUser = user.updateProfile(profile)
        return userRepository.save(updatedUser)
    }
    
    fun deleteUser(id: String) {
        if (!userRepository.existsById(id)) {
            throw EntityNotFoundException("User not found with id: $id")
        }
        userRepository.deleteById(id)
    }
    
    // 벌크 연산
    fun updateUsersStatus(userIds: List<String>, status: UserStatus): Long {
        val query = Query(Criteria.where("id").`in`(userIds))
        val update = Update()
            .set("status", status)
            .set("updated_at", LocalDateTime.now())
        
        val result = mongoTemplate.updateMulti(query, update, UserDocument::class.java)
        return result.modifiedCount
    }
    
    // 집계 연산 예제
    @Transactional(readOnly = true)
    fun getUserStatistics(): UserStatistics {
        val totalUsers = userRepository.count()
        val activeUsers = userRepository.countByStatus(UserStatus.ACTIVE)
        val inactiveUsers = userRepository.countByStatus(UserStatus.INACTIVE)
        
        // 집계 파이프라인을 사용한 통계
        val aggregation = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("age").ne(null)),
            Aggregation.group()
                .avg("age").`as`("avgAge")
                .min("age").`as`("minAge")
                .max("age").`as`("maxAge")
        )
        
        val ageStats = mongoTemplate.aggregate(aggregation, "users", AgeStatistics::class.java)
            .uniqueMappedResult ?: AgeStatistics()
        
        return UserStatistics(
            totalUsers = totalUsers,
            activeUsers = activeUsers,
            inactiveUsers = inactiveUsers,
            ageStatistics = ageStats
        )
    }
}

data class UserStatistics(
    val totalUsers: Long,
    val activeUsers: Long,
    val inactiveUsers: Long,
    val ageStatistics: AgeStatistics
)

data class AgeStatistics(
    val avgAge: Double = 0.0,
    val minAge: Int = 0,
    val maxAge: Int = 0
)
```

#### 5. 고급 MongoDB 기능

**집계 파이프라인 활용**
```kotlin
@Service
class PostAnalyticsService(
    private val mongoTemplate: MongoTemplate
) {
    
    // 복잡한 집계 쿼리 예제
    fun getPostAnalytics(): PostAnalytics {
        // 1. 카테고리별 포스트 수와 평균 조회수
        val categoryStats = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("published").`is`(true)),
            Aggregation.group("category")
                .count().`as`("postCount")
                .avg("meta.view_count").`as`("avgViews")
                .sum("meta.view_count").`as`("totalViews")
                .first("category").`as`("category"),
            Aggregation.sort(Sort.Direction.DESC, "totalViews")
        )
        
        val categoryResults = mongoTemplate.aggregate(
            categoryStats, "posts", CategoryStats::class.java
        ).mappedResults
        
        // 2. 월별 포스트 발행 트렌드
        val monthlyTrend = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("published").`is`(true)),
            Aggregation.project()
                .and("published_at").extractYear().`as`("year")
                .and("published_at").extractMonth().`as`("month")
                .andInclude("title"),
            Aggregation.group("year", "month")
                .count().`as`("postCount"),
            Aggregation.sort(Sort.Direction.DESC, "_id.year", "_id.month")
        )
        
        val trendResults = mongoTemplate.aggregate(
            monthlyTrend, "posts", MonthlyTrend::class.java
        ).mappedResults
        
        // 3. 인기 태그 분석
        val popularTags = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("published").`is`(true)),
            Aggregation.unwind("tags"),
            Aggregation.group("tags")
                .count().`as`("count")
                .first("tags").`as`("tag"),
            Aggregation.sort(Sort.Direction.DESC, "count"),
            Aggregation.limit(10)
        )
        
        val tagResults = mongoTemplate.aggregate(
            popularTags, "posts", TagStats::class.java
        ).mappedResults
        
        return PostAnalytics(
            categoryStats = categoryResults,
            monthlyTrend = trendResults,
            popularTags = tagResults
        )
    }
    
    // 사용자별 활동 분석
    fun getUserActivity(userId: String): UserActivity {
        val userPosts = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("author_id").`is`(userId)),
            Aggregation.group()
                .count().`as`("totalPosts")
                .sum("meta.view_count").`as`("totalViews")
                .sum("meta.like_count").`as`("totalLikes")
                .sum("meta.comment_count").`as`("totalComments")
        )
        
        val activity = mongoTemplate.aggregate(
            userPosts, "posts", UserActivity::class.java
        ).uniqueMappedResult ?: UserActivity()
        
        return activity
    }
}

data class PostAnalytics(
    val categoryStats: List<CategoryStats>,
    val monthlyTrend: List<MonthlyTrend>,
    val popularTags: List<TagStats>
)

data class CategoryStats(
    val category: String,
    val postCount: Long,
    val avgViews: Double,
    val totalViews: Long
)

data class MonthlyTrend(
    val year: Int,
    val month: Int,
    val postCount: Long
)

data class TagStats(
    val tag: String,
    val count: Long
)

data class UserActivity(
    val totalPosts: Long = 0,
    val totalViews: Long = 0,
    val totalLikes: Long = 0,
    val totalComments: Long = 0
)
```

**MongoDB 트랜잭션 사용**
```kotlin
@Service
@Transactional
class UserPostService(
    private val userRepository: UserRepository,
    private val postRepository: PostRepository,
    private val mongoTemplate: MongoTemplate
) {
    
    // MongoDB 트랜잭션을 사용한 복합 작업
    @Transactional
    fun createUserAndFirstPost(
        userRequest: CreateUserRequest,
        postRequest: CreatePostRequest
    ): Pair<UserDocument, PostDocument> {
        
        // 1. 사용자 생성
        val user = UserDocument(
            name = userRequest.name,
            email = userRequest.email,
            age = userRequest.age
        )
        val savedUser = userRepository.save(user)
        
        // 2. 첫 번째 포스트 생성
        val post = PostDocument(
            title = postRequest.title,
            content = postRequest.content,
            authorId = savedUser.id!!,
            authorName = savedUser.name,
            category = postRequest.category
        )
        val savedPost = postRepository.save(post)
        
        // 3. 사용자에게 "author" 태그 추가
        val updatedUser = savedUser.addTag("author")
        userRepository.save(updatedUser)
        
        return Pair(updatedUser, savedPost)
    }
    
    // 세션 기반 트랜잭션 (더 세밀한 제어)
    fun transferPostOwnership(fromUserId: String, toUserId: String, postId: String) {
        mongoTemplate.execute { session ->
            session.withTransaction {
                // 1. 포스트 소유권 변경
                val query = Query(Criteria.where("id").`is`(postId))
                val update = Update()
                    .set("author_id", toUserId)
                    .set("updated_at", LocalDateTime.now())
                
                mongoTemplate.updateFirst(query, update, PostDocument::class.java)
                
                // 2. 이전 소유자의 포스트 수 감소 (메타데이터가 있다면)
                // 3. 새 소유자의 포스트 수 증가
                // ... 추가 로직
                
                null // 반환값
            }
        }
    }
}
```

#### 6. 성능 최적화

**인덱스 설정**
```kotlin
@Configuration
class MongoIndexConfig(
    private val mongoTemplate: MongoTemplate
) {
    
    @PostConstruct
    fun initIndexes() {
        // Users 컬렉션 인덱스
        mongoTemplate.indexOps(UserDocument::class.java).apply {
            // 이메일 유니크 인덱스
            ensureIndex(
                Index().on("email", Sort.Direction.ASC).unique()
            )
            
            // 상태별 조회 인덱스
            ensureIndex(
                Index().on("status", Sort.Direction.ASC)
            )
            
            // 태그 검색 인덱스
            ensureIndex(
                Index().on("tags", Sort.Direction.ASC)
            )
            
            // 생성일시 인덱스 (TTL 설정 가능)
            ensureIndex(
                Index().on("created_at", Sort.Direction.DESC)
            )
            
            // 복합 인덱스
            ensureIndex(
                Index()
                    .on("status", Sort.Direction.ASC)
                    .on("created_at", Sort.Direction.DESC)
            )
        }
        
        // Posts 컬렉션 인덱스
        mongoTemplate.indexOps(PostDocument::class.java).apply {
            // 카테고리별 인덱스
            ensureIndex(Index().on("category", Sort.Direction.ASC))
            
            // 작성자별 인덱스
            ensureIndex(Index().on("author_id", Sort.Direction.ASC))
            
            // 발행일시 인덱스
            ensureIndex(Index().on("published_at", Sort.Direction.DESC))
            
            // 텍스트 검색 인덱스
            ensureIndex(
                Index()
                    .on("title", Sort.Direction.ASC)
                    .on("content", Sort.Direction.ASC)
                    .named("text_search")
            )
            
            // 복합 인덱스 (발행된 포스트의 카테고리별 정렬)
            ensureIndex(
                Index()
                    .on("published", Sort.Direction.ASC)
                    .on("category", Sort.Direction.ASC)
                    .on("published_at", Sort.Direction.DESC)
            )
        }
    }
}
```

**쿼리 최적화 팁**
```kotlin
@Service
class OptimizedQueryService(
    private val mongoTemplate: MongoTemplate
) {
    
    // ✅ 좋은 예: 필요한 필드만 프로젝션
    fun getUserBasicInfo(userId: String): UserBasicInfo? {
        val query = Query(Criteria.where("id").`is`(userId))
        query.fields()
            .include("name")
            .include("email")
            .include("status")
        
        return mongoTemplate.findOne(query, UserBasicInfo::class.java, "users")
    }
    
    // ✅ 좋은 예: 인덱스를 활용한 정렬
    fun getRecentActiveUsers(limit: Int): List<UserDocument> {
        val query = Query(Criteria.where("status").`is`(UserStatus.ACTIVE))
        query.with(Sort.by(Sort.Direction.DESC, "created_at"))
        query.limit(limit)
        
        return mongoTemplate.find(query, UserDocument::class.java)
    }
    
    // ✅ 좋은 예: 벌크 연산 사용
    fun markUsersAsInactive(userIds: List<String>): Long {
        val query = Query(Criteria.where("id").`in`(userIds))
        val update = Update()
            .set("status", UserStatus.INACTIVE)
            .set("updated_at", LocalDateTime.now())
        
        return mongoTemplate.updateMulti(query, update, UserDocument::class.java).modifiedCount
    }
    
    // ❌ 나쁜 예: 전체 컬렉션 스캔
    fun getAllUsersWithoutPaging(): List<UserDocument> {
        return mongoTemplate.findAll(UserDocument::class.java) // 위험!
    }
    
    // ✅ 좋은 예: 페이징과 제한 사용
    fun getUsersWithPaging(page: Int, size: Int): List<UserDocument> {
        val pageable = PageRequest.of(page, size, Sort.by("created_at").descending())
        val query = Query().with(pageable)
        
        return mongoTemplate.find(query, UserDocument::class.java)
    }
}

data class UserBasicInfo(
    val name: String,
    val email: String,
    val status: UserStatus
)
```

#### 7. MongoDB 개발/운영 팁

**개발 환경 설정**
```bash
# Docker로 MongoDB 실행
docker run -d --name mongodb -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password mongo:7

# MongoDB Compass 연결
mongodb://admin:password@localhost:27017/

# MongoDB CLI 접속
docker exec -it mongodb mongosh

# 기본 명령어
show dbs
use appdb
db.users.find()
db.users.createIndex({"email": 1}, {"unique": true})
```

**성능 모니터링**
```kotlin
@Component
class MongoHealthIndicator(
    private val mongoTemplate: MongoTemplate
) : HealthIndicator {
    
    override fun health(): Health {
        return try {
            val dbStats = mongoTemplate.execute { db ->
                db.runCommand(Document("dbStats", 1))
            }
            
            Health.up()
                .withDetail("mongodb", "Available")
                .withDetail("database", dbStats?.getString("db"))
                .withDetail("collections", dbStats?.getInteger("collections"))
                .withDetail("dataSize", dbStats?.get("dataSize"))
                .build()
        } catch (e: Exception) {
            Health.down()
                .withDetail("mongodb", "Not Available")
                .withException(e)
                .build()
        }
    }
}
```

**베스트 프랙티스**
- **문서 설계**: 임베디드 vs 참조 전략을 신중히 선택
- **인덱스 전략**: 쿼리 패턴에 맞는 인덱스 설계
- **데이터 모델링**: 읽기 패턴에 최적화된 스키마 설계
- **집계 파이프라인**: 복잡한 분석은 집계 활용
- **트랜잭션**: 필요한 경우에만 사용 (성능 고려)
- **샤딩**: 대용량 데이터의 경우 샤딩 전략 고려

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

## 프론트엔드 상세 코딩 가이드

### React.js 코딩 스타일

#### 컴포넌트 명명 규칙
```javascript
// ✅ 좋은 예: PascalCase 사용
const UserProfile = () => {
  return <div>User Profile</div>;
};

// ✅ 파일명도 PascalCase
// UserProfile.jsx

// ❌ 나쁜 예: camelCase 사용
const userProfile = () => {
  return <div>User Profile</div>;
};
```

#### 함수형 컴포넌트 vs 클래스형 컴포넌트
```javascript
// ✅ 권장: 함수형 컴포넌트 + Hooks 사용
const UserList = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    fetchUsers();
  }, []);

  const fetchUsers = async () => {
    setLoading(true);
    try {
      const response = await userApi.getUsers();
      setUsers(response.data);
    } catch (error) {
      console.error('Failed to fetch users:', error);
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <div>Loading...</div>;

  return (
    <div className="user-list">
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};

// ❌ 지양: 클래스형 컴포넌트 (특별한 경우 제외)
class UserList extends Component {
  // 구식 방식
}
```

### 컴포넌트 설계 원칙

#### 1. Single Responsibility Principle (단일 책임 원칙)
```javascript
// ✅ 좋은 예: 각각의 책임이 명확
const UserCard = ({ user, onEdit, onDelete }) => {
  return (
    <div className="user-card">
      <UserAvatar src={user.avatar} alt={user.name} />
      <UserInfo user={user} />
      <UserActions onEdit={onEdit} onDelete={onDelete} />
    </div>
  );
};

const UserAvatar = ({ src, alt }) => (
  <img src={src} alt={alt} className="user-avatar" />
);

const UserInfo = ({ user }) => (
  <div className="user-info">
    <h3>{user.name}</h3>
    <p>{user.email}</p>
  </div>
);

const UserActions = ({ onEdit, onDelete }) => (
  <div className="user-actions">
    <button onClick={onEdit}>Edit</button>
    <button onClick={onDelete}>Delete</button>
  </div>
);

// ❌ 나쁜 예: 하나의 컴포넌트에 모든 책임
const UserCard = ({ user, onEdit, onDelete }) => {
  return (
    <div className="user-card">
      <img src={user.avatar} alt={user.name} className="user-avatar" />
      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
      <div className="user-actions">
        <button onClick={onEdit}>Edit</button>
        <button onClick={onDelete}>Delete</button>
      </div>
      {/* 너무 많은 로직이 한 곳에... */}
    </div>
  );
};
```

#### 2. Props 설계 및 검증
```javascript
import PropTypes from 'prop-types';

// ✅ 좋은 예: 명확한 Props 인터페이스
const UserProfile = ({ 
  user, 
  isEditing, 
  onEdit, 
  onSave, 
  onCancel 
}) => {
  return (
    <div className="user-profile">
      {isEditing ? (
        <UserEditForm 
          user={user} 
          onSave={onSave} 
          onCancel={onCancel} 
        />
      ) : (
        <UserDisplay 
          user={user} 
          onEdit={onEdit} 
        />
      )}
    </div>
  );
};

// Props 검증
UserProfile.propTypes = {
  user: PropTypes.shape({
    id: PropTypes.number.isRequired,
    name: PropTypes.string.isRequired,
    email: PropTypes.string.isRequired,
    avatar: PropTypes.string
  }).isRequired,
  isEditing: PropTypes.bool,
  onEdit: PropTypes.func.isRequired,
  onSave: PropTypes.func.isRequired,
  onCancel: PropTypes.func.isRequired
};

// 기본값 설정
UserProfile.defaultProps = {
  isEditing: false
};
```

### Hooks 사용 가이드

#### 1. useState 최적화
```javascript
// ✅ 좋은 예: 관련된 상태를 객체로 그룹화
const UserForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    phone: ''
  });
  
  const [formState, setFormState] = useState({
    loading: false,
    errors: {},
    isValid: false
  });

  const handleInputChange = (field, value) => {
    setFormData(prev => ({
      ...prev,
      [field]: value
    }));
  };

  const validateForm = () => {
    const errors = {};
    if (!formData.name.trim()) errors.name = 'Name is required';
    if (!formData.email.trim()) errors.email = 'Email is required';
    
    setFormState(prev => ({
      ...prev,
      errors,
      isValid: Object.keys(errors).length === 0
    }));
  };

  return (
    <form>
      <input
        value={formData.name}
        onChange={(e) => handleInputChange('name', e.target.value)}
        onBlur={validateForm}
      />
      {formState.errors.name && (
        <span className="error">{formState.errors.name}</span>
      )}
    </form>
  );
};

// ❌ 나쁜 예: 너무 많은 개별 state
const UserForm = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [phone, setPhone] = useState('');
  const [loading, setLoading] = useState(false);
  const [nameError, setNameError] = useState('');
  const [emailError, setEmailError] = useState('');
  // 상태가 흩어져서 관리가 어려움
};
```

#### 2. useEffect 최적화
```javascript
// ✅ 좋은 예: 의존성 배열 명시적 관리
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);

  // 사용자 데이터 로딩
  useEffect(() => {
    const fetchUser = async () => {
      setLoading(true);
      try {
        const userData = await userApi.getUser(userId);
        setUser(userData);
      } catch (error) {
        console.error('Failed to fetch user:', error);
      } finally {
        setLoading(false);
      }
    };

    if (userId) {
      fetchUser();
    }
  }, [userId]); // userId가 변경될 때만 실행

  // 클린업 함수 사용
  useEffect(() => {
    const timer = setInterval(() => {
      // 주기적 업데이트
    }, 5000);

    return () => clearInterval(timer); // 클린업
  }, []);

  return loading ? <div>Loading...</div> : <UserDetails user={user} />;
};

// ❌ 나쁜 예: 의존성 배열 누락
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    userApi.getUser(userId).then(setUser);
  }); // 의존성 배열 없음 - 무한 루프!
};
```

#### 3. 커스텀 Hook 활용
```javascript
// ✅ 재사용 가능한 커스텀 Hook
const useApi = (apiCall, dependencies = []) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const execute = useCallback(async (...args) => {
    setLoading(true);
    setError(null);
    
    try {
      const result = await apiCall(...args);
      setData(result);
      return result;
    } catch (err) {
      setError(err);
      throw err;
    } finally {
      setLoading(false);
    }
  }, dependencies);

  return { data, loading, error, execute };
};

// 사용 예시
const UserList = () => {
  const { 
    data: users, 
    loading, 
    error, 
    execute: fetchUsers 
  } = useApi(userApi.getUsers);

  useEffect(() => {
    fetchUsers();
  }, [fetchUsers]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      {users?.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};

// Form 관리용 커스텀 Hook
const useForm = (initialValues, validationRules) => {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});

  const setValue = (field, value) => {
    setValues(prev => ({ ...prev, [field]: value }));
  };

  const setTouched = (field) => {
    setTouched(prev => ({ ...prev, [field]: true }));
  };

  const validate = () => {
    const newErrors = {};
    Object.keys(validationRules).forEach(field => {
      const rule = validationRules[field];
      const value = values[field];
      
      if (rule.required && (!value || value.trim() === '')) {
        newErrors[field] = `${field} is required`;
      } else if (rule.pattern && !rule.pattern.test(value)) {
        newErrors[field] = rule.message;
      }
    });
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const reset = () => {
    setValues(initialValues);
    setErrors({});
    setTouched({});
  };

  return {
    values,
    errors,
    touched,
    setValue,
    setTouched,
    validate,
    reset,
    isValid: Object.keys(errors).length === 0
  };
};
```

### 상태 관리 패턴

#### 1. Local State vs Global State
```javascript
// ✅ Local State: 컴포넌트 내부에서만 사용
const UserCard = ({ user }) => {
  const [isExpanded, setIsExpanded] = useState(false);
  const [isEditing, setIsEditing] = useState(false);

  return (
    <div className="user-card">
      <button onClick={() => setIsExpanded(!isExpanded)}>
        {isExpanded ? 'Collapse' : 'Expand'}
      </button>
      {isExpanded && <UserDetails user={user} />}
    </div>
  );
};

// ✅ Global State: Context API 사용
const UserContext = createContext();

const UserProvider = ({ children }) => {
  const [currentUser, setCurrentUser] = useState(null);
  const [users, setUsers] = useState([]);

  const login = async (credentials) => {
    const user = await authApi.login(credentials);
    setCurrentUser(user);
  };

  const logout = () => {
    setCurrentUser(null);
  };

  return (
    <UserContext.Provider value={{
      currentUser,
      users,
      login,
      logout,
      setUsers
    }}>
      {children}
    </UserContext.Provider>
  );
};

// 사용
const useUser = () => {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
};
```

#### 2. Redux Toolkit 사용 (복잡한 상태 관리)
```javascript
// store/userSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// 비동기 액션
export const fetchUsers = createAsyncThunk(
  'users/fetchUsers',
  async (_, { rejectWithValue }) => {
    try {
      const response = await userApi.getUsers();
      return response.data;
    } catch (error) {
      return rejectWithValue(error.response.data);
    }
  }
);

const userSlice = createSlice({
  name: 'users',
  initialState: {
    users: [],
    currentUser: null,
    loading: false,
    error: null
  },
  reducers: {
    setCurrentUser: (state, action) => {
      state.currentUser = action.payload;
    },
    clearError: (state) => {
      state.error = null;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.users = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});

export const { setCurrentUser, clearError } = userSlice.actions;
export default userSlice.reducer;

// 컴포넌트에서 사용
const UserList = () => {
  const dispatch = useDispatch();
  const { users, loading, error } = useSelector(state => state.users);

  useEffect(() => {
    dispatch(fetchUsers());
  }, [dispatch]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};
```

### 성능 최적화

#### 1. React.memo와 useMemo, useCallback
```javascript
// ✅ React.memo로 불필요한 리렌더링 방지
const UserCard = React.memo(({ user, onEdit, onDelete }) => {
  console.log('UserCard rendered for:', user.name);
  
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onEdit(user.id)}>Edit</button>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
});

// Props 비교 함수 (선택사항)
const UserCard = React.memo(({ user, onEdit, onDelete }) => {
  // 컴포넌트 내용
}, (prevProps, nextProps) => {
  return prevProps.user.id === nextProps.user.id &&
         prevProps.user.name === nextProps.user.name &&
         prevProps.user.email === nextProps.user.email;
});

// ✅ useMemo로 비싼 계산 최적화
const UserStatistics = ({ users }) => {
  const statistics = useMemo(() => {
    console.log('Calculating statistics...');
    return {
      totalUsers: users.length,
      activeUsers: users.filter(user => user.status === 'active').length,
      averageAge: users.reduce((sum, user) => sum + user.age, 0) / users.length
    };
  }, [users]); // users가 변경될 때만 재계산

  return (
    <div>
      <p>Total Users: {statistics.totalUsers}</p>
      <p>Active Users: {statistics.activeUsers}</p>
      <p>Average Age: {statistics.averageAge}</p>
    </div>
  );
};

// ✅ useCallback으로 함수 최적화
const UserList = () => {
  const [users, setUsers] = useState([]);
  const [filter, setFilter] = useState('');

  // 함수가 매번 새로 생성되는 것을 방지
  const handleEdit = useCallback((userId) => {
    console.log('Editing user:', userId);
    // 편집 로직
  }, []);

  const handleDelete = useCallback((userId) => {
    setUsers(prev => prev.filter(user => user.id !== userId));
  }, []);

  const filteredUsers = useMemo(() => {
    return users.filter(user => 
      user.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [users, filter]);

  return (
    <div>
      <input 
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter users..."
      />
      {filteredUsers.map(user => (
        <UserCard 
          key={user.id} 
          user={user}
          onEdit={handleEdit}
          onDelete={handleDelete}
        />
      ))}
    </div>
  );
};
```

#### 2. Lazy Loading과 Code Splitting
```javascript
// ✅ React.lazy로 컴포넌트 지연 로딩
const UserProfile = React.lazy(() => import('./UserProfile'));
const UserSettings = React.lazy(() => import('./UserSettings'));
const AdminPanel = React.lazy(() => import('./AdminPanel'));

const App = () => {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/profile" element={<UserProfile />} />
          <Route path="/settings" element={<UserSettings />} />
          <Route path="/admin" element={<AdminPanel />} />
        </Routes>
      </Suspense>
    </Router>
  );
};

// ✅ 동적 import로 조건부 로딩
const UserDashboard = () => {
  const [showChart, setShowChart] = useState(false);
  const [ChartComponent, setChartComponent] = useState(null);

  const loadChart = async () => {
    if (!ChartComponent) {
      const { default: Chart } = await import('./Chart');
      setChartComponent(() => Chart);
    }
    setShowChart(true);
  };

  return (
    <div>
      <button onClick={loadChart}>Show Chart</button>
      {showChart && ChartComponent && <ChartComponent />}
    </div>
  );
};
```

### 스타일링 가이드

#### 1. CSS Modules
```javascript
// UserCard.module.css
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin: 8px 0;
  background: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card:hover {
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 8px;
  color: #333;
}

.email {
  color: #666;
  font-size: 0.9rem;
}

.actions {
  margin-top: 12px;
  display: flex;
  gap: 8px;
}

.button {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8rem;
}

.editButton {
  background: #007bff;
  color: white;
}

.deleteButton {
  background: #dc3545;
  color: white;
}

// UserCard.jsx
import styles from './UserCard.module.css';

const UserCard = ({ user, onEdit, onDelete }) => {
  return (
    <div className={styles.card}>
      <h3 className={styles.title}>{user.name}</h3>
      <p className={styles.email}>{user.email}</p>
      <div className={styles.actions}>
        <button 
          className={`${styles.button} ${styles.editButton}`}
          onClick={() => onEdit(user.id)}
        >
          Edit
        </button>
        <button 
          className={`${styles.button} ${styles.deleteButton}`}
          onClick={() => onDelete(user.id)}
        >
          Delete
        </button>
      </div>
    </div>
  );
};
```

#### 2. Styled Components
```javascript
import styled from 'styled-components';

// ✅ 기본 스타일 컴포넌트
const Card = styled.div`
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin: 8px 0;
  background: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: box-shadow 0.2s ease;

  &:hover {
    box-shadow: 0 4px 8px rgba(0,0,0,0.15);
  }
`;

const Title = styled.h3`
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 8px;
  color: #333;
`;

const Email = styled.p`
  color: #666;
  font-size: 0.9rem;
  margin: 0;
`;

const Actions = styled.div`
  margin-top: 12px;
  display: flex;
  gap: 8px;
`;

// ✅ Props 기반 스타일링
const Button = styled.button`
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8rem;
  transition: opacity 0.2s ease;

  &:hover {
    opacity: 0.8;
  }

  ${props => props.variant === 'primary' && `
    background: #007bff;
    color: white;
  `}

  ${props => props.variant === 'danger' && `
    background: #dc3545;
    color: white;
  `}
`;

// 사용
const UserCard = ({ user, onEdit, onDelete }) => {
  return (
    <div className={styles.card}>
      <h3 className={styles.title}>{user.name}</h3>
      <p className={styles.email}>{user.email}</p>
      <div className={styles.actions}>
        <button 
          className={`${styles.button} ${styles.editButton}`}
          onClick={() => onEdit(user.id)}
        >
          Edit
        </button>
        <button 
          className={`${styles.button} ${styles.deleteButton}`}
          onClick={() => onDelete(user.id)}
        >
          Delete
        </button>
      </div>
    </div>
  );
};
```

### API 통신 패턴

#### 1. Axios 기반 API 클라이언트
```javascript
// api/client.js
import axios from 'axios';

const API_BASE_URL = process.env.REACT_APP_API_BASE_URL || 'http://localhost:8080';

const apiClient = axios.create({
  baseURL: API_BASE_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// 요청 인터셉터
apiClient.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('authToken');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// 응답 인터셉터
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem('authToken');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default apiClient;

// api/userApi.js
import apiClient from './client';

export const userApi = {
  getUsers: (params = {}) => {
    return apiClient.get('/api/v1/users', { params });
  },

  getUser: (id) => {
    return apiClient.get(`/api/v1/users/${id}`);
  },

  createUser: (userData) => {
    return apiClient.post('/api/v1/users', userData);
  },

  updateUser: (id, userData) => {
    return apiClient.put(`/api/v1/users/${id}`, userData);
  },

  deleteUser: (id) => {
    return apiClient.delete(`/api/v1/users/${id}`);
  }
};
```

#### 2. React Query 사용 (권장)
```javascript
// hooks/useUsers.js
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { userApi } from '../api/userApi';

export const useUsers = (params = {}) => {
  return useQuery({
    queryKey: ['users', params],
    queryFn: () => userApi.getUsers(params),
    select: (response) => response.data,
    staleTime: 5 * 60 * 1000, // 5분
    cacheTime: 10 * 60 * 1000, // 10분
  });
};

export const useUser = (id) => {
  return useQuery({
    queryKey: ['user', id],
    queryFn: () => userApi.getUser(id),
    select: (response) => response.data,
    enabled: !!id, // id가 있을 때만 실행
  });
};

export const useCreateUser = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: userApi.createUser,
    onSuccess: () => {
      // 사용자 목록 캐시 무효화
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
    onError: (error) => {
      console.error('Failed to create user:', error);
    }
  });
};

export const useUpdateUser = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: ({ id, ...data }) => userApi.updateUser(id, data),
    onSuccess: (_, variables) => {
      // 특정 사용자 캐시 무효화
      queryClient.invalidateQueries({ queryKey: ['user', variables.id] });
      queryClient.invalidateQueries({ queryKey: ['users'] });
    }
  });
};

// 컴포넌트에서 사용
const UserList = () => {
  const { data: users, isLoading, error } = useUsers();
  const createUserMutation = useCreateUser();

  const handleCreateUser = async (userData) => {
    try {
      await createUserMutation.mutateAsync(userData);
      // 성공 메시지 표시
    } catch (error) {
      // 에러 메시지 표시
    }
  };

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      {users?.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};
```

### 에러 처리 패턴

#### 1. Error Boundary
```javascript
// components/ErrorBoundary.jsx
class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // 에러 로깅 서비스로 전송
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Something went wrong.</h2>
          <details>
            {this.state.error && this.state.error.toString()}
          </details>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// App.jsx에서 사용
const App = () => {
  return (
    <ErrorBoundary>
      <Router>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/users" element={<UserList />} />
        </Routes>
      </Router>
    </ErrorBoundary>
  );
};
```

#### 2. 글로벌 에러 처리
```javascript
// context/ErrorContext.jsx
const ErrorContext = createContext();

export const ErrorProvider = ({ children }) => {
  const [errors, setErrors] = useState([]);

  const addError = (error) => {
    const errorId = Date.now();
    setErrors(prev => [...prev, { id: errorId, message: error.message }]);
    
    // 5초 후 자동 제거
    setTimeout(() => {
      removeError(errorId);
    }, 5000);
  };

  const removeError = (id) => {
    setErrors(prev => prev.filter(error => error.id !== id));
  };

  return (
    <ErrorContext.Provider value={{ errors, addError, removeError }}>
      {children}
    </ErrorContext.Provider>
  );
};

// components/ErrorToast.jsx
const ErrorToast = ({ errors, onRemove }) => {
  return (
    <div className="error-toast-container">
      {errors.map(error => (
        <div key={error.id} className="error-toast">
          <span>{error.message}</span>
          <button onClick={() => onRemove(error.id)}>×</button>
        </div>
      ))}
    </div>
  );
};
```

### 테스트 작성 가이드

#### 1. 컴포넌트 테스트 (React Testing Library)
```javascript
// UserCard.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import { UserCard } from './UserCard';

const mockUser = {
  id: 1,
  name: 'John Doe',
  email: 'john@example.com'
};

describe('UserCard', () => {
  it('renders user information correctly', () => {
    render(
      <UserCard 
        user={mockUser} 
        onEdit={() => {}} 
        onDelete={() => {}} 
      />
    );

    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });

  it('calls onEdit when edit button is clicked', () => {
    const onEdit = jest.fn();
    
    render(
      <UserCard 
        user={mockUser} 
        onEdit={onEdit} 
        onDelete={() => {}} 
      />
    );

    fireEvent.click(screen.getByText('Edit'));
    expect(onEdit).toHaveBeenCalledWith(1);
  });

  it('calls onDelete when delete button is clicked', () => {
    const onDelete = jest.fn();
    
    render(
      <UserCard 
        user={mockUser} 
        onEdit={() => {}} 
        onDelete={onDelete} 
      />
    );

    fireEvent.click(screen.getByText('Delete'));
    expect(onDelete).toHaveBeenCalledWith(1);
  });
});
```

#### 2. 커스텀 Hook 테스트
```javascript
// useUsers.test.js
import { renderHook, waitFor } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { useUsers } from './useUsers';
import { userApi } from '../api/userApi';

jest.mock('../api/userApi');

const createWrapper = () => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  });

  return ({ children }) => (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  );
};

describe('useUsers', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('fetches users successfully', async () => {
    const mockUsers = [
      { id: 1, name: 'John Doe', email: 'john@example.com' }
    ];
    
    userApi.getUsers.mockResolvedValue({ data: mockUsers });

    const { result } = renderHook(() => useUsers(), {
      wrapper: createWrapper(),
    });

    await waitFor(() => {
      expect(result.current.isSuccess).toBe(true);
    });

    expect(result.current.data).toEqual(mockUsers);
  });

  it('handles error correctly', async () => {
    userApi.getUsers.mockRejectedValue(new Error('API Error'));

    const { result } = renderHook(() => useUsers(), {
      wrapper: createWrapper(),
    });

    await waitFor(() => {
      expect(result.current.isError).toBe(true);
    });

    expect(result.current.error.message).toBe('API Error');
  });
});
```

### 실무 팁 및 베스트 프랙티스

#### 1. 폴더 구조 권장안
```
src/
├── components/          # 재사용 가능한 컴포넌트
│   ├── ui/             # 기본 UI 컴포넌트 (Button, Input 등)
│   ├── layout/         # 레이아웃 컴포넌트
│   └── common/         # 공통 컴포넌트
├── pages/              # 페이지 컴포넌트
├── hooks/              # 커스텀 Hook
├── context/            # Context 제공자
├── api/                # API 관련
├── utils/              # 유틸리티 함수
├── constants/          # 상수
├── styles/             # 글로벌 스타일
└── __tests__/          # 테스트 파일
```

#### 2. 환경 변수 관리
```javascript
// .env.development
REACT_APP_API_BASE_URL=http://localhost:8080
REACT_APP_APP_NAME=Dev App

// .env.production
REACT_APP_API_BASE_URL=https://api.example.com
REACT_APP_APP_NAME=Production App

// config/env.js
export const config = {
  apiBaseUrl: process.env.REACT_APP_API_BASE_URL,
  appName: process.env.REACT_APP_APP_NAME,
  isDevelopment: process.env.NODE_ENV === 'development',
  isProduction: process.env.NODE_ENV === 'production',
};
```

#### 3. 타입 안정성 (PropTypes 또는 TypeScript)
```javascript
// TypeScript 사용 시 (권장)
interface User {
  id: number;
  name: string;
  email: string;
  avatar?: string;
}

interface UserCardProps {
  user: User;
  onEdit: (id: number) => void;
  onDelete: (id: number) => void;
}

const UserCard: React.FC<UserCardProps> = ({ user, onEdit, onDelete }) => {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onEdit(user.id)}>Edit</button>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
};
```

## 테스트 가이드라인

### 테스트 전략 및 종류

#### 테스트 피라미드
```
         /\
        /  \
       /E2E \     <- 소수의 핵심 플로우
      /______\
     /        \
    / 통합 테스트 \   <- 주요 모듈 연동
   /____________\
  /              \
 /    단위 테스트    \  <- 대부분의 테스트
/__________________\
```

**1. 단위 테스트 (Unit Tests)**
- **목적**: 개별 함수, 메소드, 컴포넌트의 정확성 검증
- **비율**: 전체 테스트의 70%
- **특징**: 빠르고, 독립적이며, 단순함

**2. 통합 테스트 (Integration Tests)**
- **목적**: 여러 모듈/컴포넌트 간의 상호작용 검증
- **비율**: 전체 테스트의 20%
- **특징**: 실제 데이터베이스나 API와의 연동 테스트

**3. E2E 테스트 (End-to-End Tests)**
- **목적**: 사용자 관점에서 전체 애플리케이션 플로우 검증
- **비율**: 전체 테스트의 10%
- **특징**: 실제 브라우저 환경에서 테스트

### 백엔드 테스트 상세 가이드

#### 1. 테스트 환경 설정

**build.gradle.kts 설정**
```kotlin
dependencies {
    testImplementation("org.springframework.boot:spring-boot-starter-test")
    testImplementation("org.springframework.boot:spring-boot-testcontainers")
    testImplementation("org.testcontainers:junit-jupiter")
    testImplementation("org.testcontainers:postgresql")
    testImplementation("io.mockk:mockk:1.13.8")
    testImplementation("com.ninja-squad:springmockk:4.0.2")
}
```

**application-test.yml**
```yaml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driver-class-name: org.h2.Driver
    username: sa
    password:
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: true
  profiles:
    active: test

logging:
  level:
    com.company: DEBUG
    org.springframework.web: DEBUG
```

#### 2. 단위 테스트 작성

**도메인 엔티티 테스트**
```kotlin
// UserTest.kt
@DisplayName("User 엔티티 테스트")
class UserTest {
    
    @Test
    @DisplayName("사용자 생성 - 성공")
    fun `should create user with valid data`() {
        // Given
        val email = Email("test@example.com")
        val name = "홍길동"
        
        // When
        val user = User.create(name, email)
        
        // Then
        assertThat(user.name).isEqualTo(name)
        assertThat(user.email).isEqualTo(email)
        assertThat(user.status).isEqualTo(UserStatus.ACTIVE)
        assertThat(user.flagActive()).isTrue()
    }
    
    @Test
    @DisplayName("사용자 생성 - 빈 이름으로 실패")
    fun `should fail to create user with empty name`() {
        // Given
        val email = Email("test@example.com")
        val emptyName = ""
        
        // When & Then
        assertThatThrownBy {
            User.create(emptyName, email)
        }.isInstanceOf(IllegalArgumentException::class.java)
         .hasMessage("Name cannot be blank")
    }
    
    @Test
    @DisplayName("사용자 상태 변경")
    fun `should change user status`() {
        // Given
        val user = User.create("홍길동", Email("test@example.com"))
        
        // When
        user.deactivate()
        
        // Then
        assertThat(user.status).isEqualTo(UserStatus.INACTIVE)
        assertThat(user.flagActive()).isFalse()
    }
}
```

**도메인 서비스 테스트**
```kotlin
// UserDomainServiceTest.kt
@ExtendWith(MockKExtension::class)
@DisplayName("User 도메인 서비스 테스트")
class UserDomainServiceTest {
    
    @MockK
    private lateinit var userRepository: UserRepository
    
    private lateinit var userDomainService: UserDomainService
    
    @BeforeEach
    fun setUp() {
        userDomainService = UserDomainService(userRepository)
    }
    
    @Test
    @DisplayName("이메일 중복 검증 - 중복 없음")
    fun `should validate email is not duplicated`() {
        // Given
        val email = Email("new@example.com")
        every { userRepository.existsByEmail(email) } returns false
        
        // When
        val result = userDomainService.validateEmailUniqueness(email)
        
        // Then
        assertThat(result).isTrue()
        verify { userRepository.existsByEmail(email) }
    }
    
    @Test
    @DisplayName("이메일 중복 검증 - 중복 존재")
    fun `should throw exception when email is duplicated`() {
        // Given
        val email = Email("existing@example.com")
        every { userRepository.existsByEmail(email) } returns true
        
        // When & Then
        assertThatThrownBy {
            userDomainService.validateEmailUniqueness(email)
        }.isInstanceOf(DuplicateEmailException::class.java)
         .hasMessage("Email already exists: ${email.value}")
    }
}
```

**애플리케이션 서비스 테스트**
```kotlin
// UserApplicationServiceTest.kt
@ExtendWith(MockKExtension::class)
@DisplayName("User 애플리케이션 서비스 테스트")
class UserApplicationServiceTest {
    
    @MockK
    private lateinit var userRepository: UserRepository
    
    @MockK
    private lateinit var userDomainService: UserDomainService
    
    private lateinit var userApplicationService: UserApplicationService
    
    @BeforeEach
    fun setUp() {
        userApplicationService = UserApplicationService(
            userRepository, 
            userDomainService
        )
    }
    
    @Test
    @DisplayName("사용자 생성 - 성공")
    fun `should create user successfully`() {
        // Given
        val request = CreateUserRequest("홍길동", "test@example.com")
        val user = User.create(request.name, Email(request.email))
        
        every { userDomainService.validateEmailUniqueness(any()) } returns true
        every { userRepository.save(any()) } returns user.copy(id = 1L)
        
        // When
        val response = userApplicationService.createUser(request)
        
        // Then
        assertThat(response.id).isEqualTo(1L)
        assertThat(response.name).isEqualTo(request.name)
        assertThat(response.email).isEqualTo(request.email)
        
        verify { userDomainService.validateEmailUniqueness(Email(request.email)) }
        verify { userRepository.save(any()) }
    }
    
    @Test
    @DisplayName("사용자 조회 - 존재하지 않는 사용자")
    fun `should throw exception when user not found`() {
        // Given
        val userId = 999L
        every { userRepository.findById(userId) } returns null
        
        // When & Then
        assertThatThrownBy {
            userApplicationService.getUser(userId)
        }.isInstanceOf(UserNotFoundException::class.java)
         .hasMessage("User not found with id: $userId")
    }
}
```

#### 3. 통합 테스트 작성

**Repository 통합 테스트**
```kotlin
// UserRepositoryIntegrationTest.kt
@DataJpaTest
@TestPropertySource(properties = ["spring.jpa.hibernate.ddl-auto=create-drop"])
@DisplayName("User Repository 통합 테스트")
class UserRepositoryIntegrationTest @Autowired constructor(
    private val userRepository: UserRepository,
    private val testEntityManager: TestEntityManager
) {
    
    @Test
    @DisplayName("이메일로 사용자 조회")
    fun `should find user by email`() {
        // Given
        val user = User.create("홍길동", Email("test@example.com"))
        testEntityManager.persistAndFlush(user)
        
        // When
        val foundUser = userRepository.findByEmail(Email("test@example.com"))
        
        // Then
        assertThat(foundUser).isNotNull
        assertThat(foundUser?.name).isEqualTo("홍길동")
        assertThat(foundUser?.email?.value).isEqualTo("test@example.com")
    }
    
    @Test
    @DisplayName("활성 사용자 목록 조회")
    fun `should find only active users`() {
        // Given
        val activeUser1 = User.create("홍길동", Email("active1@example.com"))
        val activeUser2 = User.create("김철수", Email("active2@example.com"))
        val inactiveUser = User.create("이영희", Email("inactive@example.com"))
        inactiveUser.deactivate()
        
        testEntityManager.persistAndFlush(activeUser1)
        testEntityManager.persistAndFlush(activeUser2)
        testEntityManager.persistAndFlush(inactiveUser)
        
        // When
        val activeUsers = userRepository.findByStatus(UserStatus.ACTIVE)
        
        // Then
        assertThat(activeUsers).hasSize(2)
        assertThat(activeUsers.map { it.name }).containsExactlyInAnyOrder("홍길동", "김철수")
    }
}
```

**Controller 통합 테스트**
```kotlin
// UserControllerIntegrationTest.kt
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@TestPropertySource(properties = ["spring.profiles.active=test"])
@Transactional
@DisplayName("User Controller 통합 테스트")
class UserControllerIntegrationTest @Autowired constructor(
    private val restTemplate: TestRestTemplate,
    private val userRepository: UserRepository
) {
    
    @Test
    @DisplayName("사용자 생성 API - 성공")
    fun `should create user via API`() {
        // Given
        val request = CreateUserRequest("홍길동", "test@example.com")
        val entity = HttpEntity(request)
        
        // When
        val response = restTemplate.postForEntity(
            "/api/v1/users", 
            entity, 
            ApiResponse::class.java
        )
        
        // Then
        assertThat(response.statusCode).isEqualTo(HttpStatus.CREATED)
        
        val savedUser = userRepository.findByEmail(Email("test@example.com"))
        assertThat(savedUser).isNotNull
        assertThat(savedUser?.name).isEqualTo("홍길동")
    }
    
    @Test
    @DisplayName("사용자 목록 조회 API - 페이징")
    fun `should get users with pagination`() {
        // Given
        repeat(25) { i ->
            val user = User.create("사용자$i", Email("user$i@example.com"))
            userRepository.save(user)
        }
        
        // When
        val response = restTemplate.getForEntity(
            "/api/v1/users?page=0&size=10", 
            String::class.java
        )
        
        // Then
        assertThat(response.statusCode).isEqualTo(HttpStatus.OK)
        // JSON 응답 검증 로직 추가
    }
}
```

#### 4. 테스트 실행 및 커버리지

**테스트 실행 명령어**
```bash
# 모든 테스트 실행
./gradlew test

# 특정 테스트 클래스 실행
./gradlew test --tests "*UserServiceTest"

# 특정 패키지의 테스트 실행
./gradlew test --tests "com.company.user.*"

# 테스트 결과를 상세히 보기
./gradlew test --info

# 병렬 테스트 실행 (성능 향상)
./gradlew test --parallel

# 특정 테스트 메소드만 실행
./gradlew test --tests "*UserServiceTest.should create user successfully"
```

**커버리지 확인**
```bash
# JaCoCo 커버리지 리포트 생성
./gradlew jacocoTestReport

# 커버리지 검증 (최소 80% 요구)
./gradlew jacocoTestCoverageVerification

# 리포트 확인 위치
# build/reports/jacoco/test/html/index.html
```

**build.gradle.kts JaCoCo 설정**
```kotlin
jacoco {
    toolVersion = "0.8.8"
}

tasks.jacocoTestReport {
    reports {
        xml.required.set(true)
        html.required.set(true)
        csv.required.set(false)
    }
    finalizedBy(tasks.jacocoTestCoverageVerification)
}

tasks.jacocoTestCoverageVerification {
    violationRules {
        rule {
            limit {
                minimum = "0.80".toBigDecimal() // 80% 커버리지 요구
            }
        }
        rule {
            element = "CLASS"
            excludes = listOf("*.config.*", "*.dto.*")
            limit {
                counter = "LINE"
                value = "COVEREDRATIO"
                minimum = "0.80".toBigDecimal()
            }
        }
    }
}
```

### 프론트엔드 테스트 상세 가이드

#### 1. 테스트 환경 설정

**package.json 설정**
```json
{
  "devDependencies": {
    "@testing-library/react": "^13.4.0",
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/user-event": "^14.4.3",
    "jest": "^27.5.1",
    "jest-environment-jsdom": "^27.5.1",
    "msw": "^0.49.3"
  },
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:ci": "jest --coverage --watchAll=false"
  }
}
```

**jest.config.js**
```javascript
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/src/setupTests.js'],
  moduleNameMapping: {
    '\\.(css|less|scss|sass)$': 'identity-obj-proxy',
    '^@/(.*)$': '<rootDir>/src/$1'
  },
  collectCoverageFrom: [
    'src/**/*.{js,jsx}',
    '!src/index.js',
    '!src/reportWebVitals.js',
    '!src/**/*.stories.js'
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};
```

**src/setupTests.js**
```javascript
import '@testing-library/jest-dom';
import { server } from './mocks/server';

// MSW 서버 설정
beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

#### 2. 컴포넌트 단위 테스트

**기본 컴포넌트 테스트**
```javascript
// UserCard.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { UserCard } from './UserCard';

const mockUser = {
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  status: 'ACTIVE'
};

describe('UserCard', () => {
  test('사용자 정보를 올바르게 렌더링한다', () => {
    // Given
    const onEdit = jest.fn();
    const onDelete = jest.fn();
    
    // When
    render(
      <UserCard 
        user={mockUser} 
        onEdit={onEdit} 
        onDelete={onDelete} 
      />
    );
    
    // Then
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /edit/i })).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /delete/i })).toBeInTheDocument();
  });
  
  test('편집 버튼 클릭 시 onEdit 콜백이 호출된다', async () => {
    // Given
    const user = userEvent.setup();
    const onEdit = jest.fn();
    const onDelete = jest.fn();
    
    render(
      <UserCard 
        user={mockUser} 
        onEdit={onEdit} 
        onDelete={onDelete} 
      />
    );
    
    // When
    await user.click(screen.getByRole('button', { name: /edit/i }));
    
    // Then
    expect(onEdit).toHaveBeenCalledTimes(1);
    expect(onEdit).toHaveBeenCalledWith(1);
  });
  
  test('삭제 버튼 클릭 시 onDelete 콜백이 호출된다', async () => {
    // Given
    const user = userEvent.setup();
    const onEdit = jest.fn();
    const onDelete = jest.fn();
    
    render(
      <UserCard 
        user={mockUser} 
        onEdit={onEdit} 
        onDelete={onDelete} 
      />
    );
    
    // When
    await user.click(screen.getByRole('button', { name: /delete/i }));
    
    // Then
    expect(onDelete).toHaveBeenCalledTimes(1);
    expect(onDelete).toHaveBeenCalledWith(1);
  });
  
  test('비활성 사용자의 경우 상태가 표시된다', () => {
    // Given
    const inactiveUser = { ...mockUser, status: 'INACTIVE' };
    
    // When
    render(
      <UserCard 
        user={inactiveUser} 
        onEdit={() => {}} 
        onDelete={() => {}} 
      />
    );
    
    // Then
    expect(screen.getByText(/inactive/i)).toBeInTheDocument();
  });
});
```

**폼 컴포넌트 테스트**
```javascript
// UserForm.test.jsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { UserForm } from './UserForm';

describe('UserForm', () => {
  test('폼 제출 시 올바른 데이터가 전달된다', async () => {
    // Given
    const user = userEvent.setup();
    const onSubmit = jest.fn();
    
    render(<UserForm onSubmit={onSubmit} />);
    
    // When
    await user.type(screen.getByLabelText(/name/i), 'John Doe');
    await user.type(screen.getByLabelText(/email/i), 'john@example.com');
    await user.click(screen.getByRole('button', { name: /submit/i }));
    
    // Then
    await waitFor(() => {
      expect(onSubmit).toHaveBeenCalledWith({
        name: 'John Doe',
        email: 'john@example.com'
      });
    });
  });
  
  test('필수 필드 누락 시 에러 메시지가 표시된다', async () => {
    // Given
    const user = userEvent.setup();
    const onSubmit = jest.fn();
    
    render(<UserForm onSubmit={onSubmit} />);
    
    // When
    await user.click(screen.getByRole('button', { name: /submit/i }));
    
    // Then
    expect(screen.getByText(/name is required/i)).toBeInTheDocument();
    expect(screen.getByText(/email is required/i)).toBeInTheDocument();
    expect(onSubmit).not.toHaveBeenCalled();
  });
  
  test('잘못된 이메일 형식 시 에러 메시지가 표시된다', async () => {
    // Given
    const user = userEvent.setup();
    const onSubmit = jest.fn();
    
    render(<UserForm onSubmit={onSubmit} />);
    
    // When
    await user.type(screen.getByLabelText(/name/i), 'John Doe');
    await user.type(screen.getByLabelText(/email/i), 'invalid-email');
    await user.click(screen.getByRole('button', { name: /submit/i }));
    
    // Then
    expect(screen.getByText(/invalid email format/i)).toBeInTheDocument();
    expect(onSubmit).not.toHaveBeenCalled();
  });
});
```

#### 3. 커스텀 Hook 테스트

```javascript
// useUsers.test.js
import { renderHook, waitFor } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { rest } from 'msw';
import { server } from '../mocks/server';
import { useUsers, useCreateUser } from './useUsers';

const createWrapper = () => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  });

  return ({ children }) => (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  );
};

describe('useUsers', () => {
  test('사용자 목록을 성공적으로 가져온다', async () => {
    // Given
    const mockUsers = [
      { id: 1, name: 'John Doe', email: 'john@example.com' },
      { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
    ];
    
    server.use(
      rest.get('/api/v1/users', (req, res, ctx) => {
        return res(ctx.json({ data: mockUsers }));
      })
    );

    // When
    const { result } = renderHook(() => useUsers(), {
      wrapper: createWrapper(),
    });

    // Then
    await waitFor(() => {
      expect(result.current.isSuccess).toBe(true);
    });

    expect(result.current.data).toEqual(mockUsers);
  });

  test('API 에러 시 에러 상태를 반환한다', async () => {
    // Given
    server.use(
      rest.get('/api/v1/users', (req, res, ctx) => {
        return res(ctx.status(500), ctx.json({ message: 'Server Error' }));
      })
    );

    // When
    const { result } = renderHook(() => useUsers(), {
      wrapper: createWrapper(),
    });

    // Then
    await waitFor(() => {
      expect(result.current.isError).toBe(true);
    });

    expect(result.current.error).toBeDefined();
  });
});

describe('useCreateUser', () => {
  test('사용자를 성공적으로 생성한다', async () => {
    // Given
    const newUser = { name: 'New User', email: 'new@example.com' };
    const createdUser = { id: 3, ...newUser };
    
    server.use(
      rest.post('/api/v1/users', (req, res, ctx) => {
        return res(ctx.status(201), ctx.json(createdUser));
      })
    );

    // When
    const { result } = renderHook(() => useCreateUser(), {
      wrapper: createWrapper(),
    });

    // Then
    await waitFor(() => {
      result.current.mutate(newUser);
    });

    await waitFor(() => {
      expect(result.current.isSuccess).toBe(true);
    });

    expect(result.current.data).toEqual(createdUser);
  });
});
```

#### 4. 통합 테스트 (페이지 단위)

```javascript
// UserListPage.test.jsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { rest } from 'msw';
import { server } from '../mocks/server';
import { UserListPage } from './UserListPage';

const renderWithProviders = (component) => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  });

  return render(
    <QueryClientProvider client={queryClient}>
      {component}
    </QueryClientProvider>
  );
};

describe('UserListPage', () => {
  test('사용자 목록 페이지가 정상적으로 렌더링된다', async () => {
    // Given
    const mockUsers = [
      { id: 1, name: 'John Doe', email: 'john@example.com' },
      { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
    ];
    
    server.use(
      rest.get('/api/v1/users', (req, res, ctx) => {
        return res(ctx.json({ data: mockUsers }));
      })
    );

    // When
    renderWithProviders(<UserListPage />);

    // Then
    expect(screen.getByText(/loading/i)).toBeInTheDocument();
    
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
      expect(screen.getByText('Jane Smith')).toBeInTheDocument();
    });
    
    expect(screen.getByText(/user list/i)).toBeInTheDocument();
  });
  
  test('새 사용자 추가 플로우가 정상 동작한다', async () => {
    // Given
    const user = userEvent.setup();
    const mockUsers = [];
    const newUser = { id: 1, name: 'New User', email: 'new@example.com' };
    
    server.use(
      rest.get('/api/v1/users', (req, res, ctx) => {
        return res(ctx.json({ data: mockUsers }));
      }),
      rest.post('/api/v1/users', (req, res, ctx) => {
        return res(ctx.status(201), ctx.json(newUser));
      })
    );

    renderWithProviders(<UserListPage />);
    
    await waitFor(() => {
      expect(screen.getByText(/no users found/i)).toBeInTheDocument();
    });

    // When
    await user.click(screen.getByRole('button', { name: /add user/i }));
    await user.type(screen.getByLabelText(/name/i), 'New User');
    await user.type(screen.getByLabelText(/email/i), 'new@example.com');
    await user.click(screen.getByRole('button', { name: /save/i }));

    // Then
    await waitFor(() => {
      expect(screen.getByText('New User')).toBeInTheDocument();
    });
  });
});
```

#### 5. 테스트 실행 및 결과 확인

**테스트 실행 명령어**
```bash
# 모든 테스트 실행
npm test

# Watch 모드로 테스트 실행 (개발 중)
npm run test:watch

# 특정 파일만 테스트
npm test UserCard.test.jsx

# 특정 테스트 패턴으로 실행
npm test -- --testNamePattern="사용자 정보"

# 커버리지와 함께 테스트 실행
npm run test:coverage

# CI 환경에서 테스트 실행
npm run test:ci
```

**테스트 결과 해석**
```bash
# 성공 예시
 PASS  src/components/UserCard.test.jsx
  UserCard
    ✓ 사용자 정보를 올바르게 렌더링한다 (25ms)
    ✓ 편집 버튼 클릭 시 onEdit 콜백이 호출된다 (15ms)
    ✓ 삭제 버튼 클릭 시 onDelete 콜백이 호출된다 (12ms)

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        2.456s
```

**커버리지 리포트 확인**
```bash
# 커버리지 리포트 위치
coverage/lcov-report/index.html

# 터미널에서 커버리지 요약 확인
npm run test:coverage

# 결과 예시
----------------------|---------|----------|---------|---------|
File                  | % Stmts | % Branch | % Funcs | % Lines |
----------------------|---------|----------|---------|---------|
All files             |   88.89 |     87.5 |     100 |   88.24 |
 src/components       |   91.67 |     87.5 |     100 |   90.91 |
  UserCard.jsx        |   91.67 |     87.5 |     100 |   90.91 |
 src/hooks            |   85.71 |       75 |     100 |   85.71 |
  useUsers.js         |   85.71 |       75 |     100 |   85.71 |
----------------------|---------|----------|---------|---------|
```

### 테스트 작성 규칙 및 베스트 프랙티스

#### 1. 명명 규칙
```javascript
// ✅ 좋은 예: 한국어로 명확한 의도 표현
describe('UserCard', () => {
  test('사용자 정보를 올바르게 렌더링한다', () => {});
  test('편집 버튼 클릭 시 onEdit 콜백이 호출된다', () => {});
  test('비활성 사용자의 경우 비활성 상태가 표시된다', () => {});
});

// ❌ 나쁜 예: 모호한 테스트 이름
describe('UserCard', () => {
  test('test1', () => {});
  test('should work', () => {});
  test('button click', () => {});
});
```

#### 2. AAA 패턴 (Arrange, Act, Assert)
```javascript
test('사용자 생성 시 올바른 데이터가 저장된다', async () => {
  // Arrange (준비)
  const userData = { name: 'John', email: 'john@example.com' };
  const mockSave = jest.fn().mockResolvedValue({ id: 1, ...userData });
  
  // Act (실행)
  const result = await userService.createUser(userData);
  
  // Assert (검증)
  expect(result.id).toBe(1);
  expect(result.name).toBe('John');
  expect(mockSave).toHaveBeenCalledWith(userData);
});
```

#### 3. 테스트 독립성 보장
```javascript
describe('UserService', () => {
  beforeEach(() => {
    // 각 테스트 전에 상태 초기화
    jest.clearAllMocks();
    localStorage.clear();
  });
  
  afterEach(() => {
    // 각 테스트 후 정리
    cleanup();
  });
});
```

#### 4. 커버리지 목표
- **최소 커버리지**: 80%
- **중요 비즈니스 로직**: 95% 이상
- **유틸리티 함수**: 100%

#### 5. CI/CD 통합
**.github/workflows/test.yml**
```yaml
name: Tests
on: [push, pull_request]

jobs:
  backend-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
      - name: Run Backend Tests
        run: ./gradlew test jacocoTestReport
      - name: Upload Coverage
        uses: codecov/codecov-action@v3

  frontend-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install Dependencies
        run: npm ci
      - name: Run Frontend Tests
        run: npm run test:ci
      - name: Upload Coverage
        uses: codecov/codecov-action@v3
```

## 배포 프로세스

### Docker 컨테이너화 전략

#### 멀티 스테이지 빌드 아키텍처
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Build Stage   │───▶│  Runtime Stage  │───▶│  Production     │
│                 │    │                 │    │  Container      │
│ • 의존성 설치      │    │ • 최소 런타임     │     │ • 최적화된 이미지   │
│ • 코드 빌드       │    │ • 보안 강화       │     │ • 자동 배포       │
│ • 테스트 실행      │    │ • 성능 최적화     │     │ • 헬스 체크       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### 백엔드 Dockerfile 구성

#### 1. Spring Boot 애플리케이션 Dockerfile
```dockerfile
# backend/Dockerfile
FROM openjdk:17-jdk-slim as builder

# 작업 디렉토리 설정
WORKDIR /app

# Gradle Wrapper와 의존성 파일 복사 (캐시 최적화)
COPY gradlew .
COPY gradle gradle
COPY build.gradle.kts .
COPY settings.gradle.kts .

# 의존성 다운로드 (Docker 레이어 캐싱 활용)
RUN chmod +x ./gradlew
RUN ./gradlew dependencies --no-daemon

# 소스 코드 복사
COPY src src

# 애플리케이션 빌드
RUN ./gradlew bootJar --no-daemon

# 런타임 스테이지
FROM openjdk:17-jre-slim

# 보안 및 성능을 위한 사용자 생성
RUN groupadd -r appgroup && useradd -r -g appgroup appuser

# 작업 디렉토리 설정
WORKDIR /app

# 빌드된 JAR 파일 복사
COPY --from=builder /app/build/libs/*.jar app.jar

# 사용자 권한 설정
RUN chown appuser:appgroup app.jar
USER appuser

# 포트 노출
EXPOSE 8080

# 헬스 체크 설정
HEALTHCHECK --interval=30s --timeout=3s --start-period=60s --retries=3 \
    CMD curl -f http://localhost:8080/actuator/health || exit 1

# JVM 최적화 및 애플리케이션 실행
ENTRYPOINT ["java", \
    "-Djava.security.egd=file:/dev/./urandom", \
    "-XX:+UseG1GC", \
    "-XX:+UseContainerSupport", \
    "-XX:MaxRAMPercentage=75.0", \
    "-Dspring.profiles.active=${SPRING_PROFILES_ACTIVE:prod}", \
    "-jar", "app.jar"]
```

#### 2. 환경별 설정 파일
```yaml
# backend/src/main/resources/application-prod.yml
spring:
  datasource:
    url: jdbc:postgresql://${DB_HOST:localhost}:${DB_PORT:5432}/${DB_NAME:appdb}
    username: ${DB_USERNAME:app}
    password: ${DB_PASSWORD}
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000

  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
        format_sql: false

  redis:
    host: ${REDIS_HOST:localhost}
    port: ${REDIS_PORT:6379}
    password: ${REDIS_PASSWORD:}
    timeout: 2000ms

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: when-authorized

logging:
  level:
    com.company: INFO
    org.springframework.security: WARN
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} - %msg%n"
  file:
    name: /app/logs/application.log

server:
  port: 8080
  shutdown: graceful
  tomcat:
    max-threads: 200
    min-spare-threads: 10
```

### 프론트엔드 Dockerfile 구성

#### 1. React.js 애플리케이션 Dockerfile
```dockerfile
# frontend/Dockerfile
FROM node:18-alpine as builder

# 작업 디렉토리 설정
WORKDIR /app

# package.json과 package-lock.json 복사 (캐시 최적화)
COPY package*.json ./

# 의존성 설치
RUN npm ci --only=production

# 소스 코드 복사
COPY . .

# 빌드 인수 설정
ARG REACT_APP_API_BASE_URL
ARG REACT_APP_ENV

# 환경 변수 설정
ENV REACT_APP_API_BASE_URL=$REACT_APP_API_BASE_URL
ENV REACT_APP_ENV=$REACT_APP_ENV

# 애플리케이션 빌드
RUN npm run build

# Nginx 런타임 스테이지
FROM nginx:alpine

# Nginx 설정 복사
COPY nginx.conf /etc/nginx/nginx.conf

# 빌드된 정적 파일 복사
COPY --from=builder /app/build /usr/share/nginx/html

# 포트 노출
EXPOSE 80

# 헬스 체크 설정
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost/ || exit 1

# Nginx 실행
CMD ["nginx", "-g", "daemon off;"]
```

#### 2. Nginx 설정 파일
```nginx
# frontend/nginx.conf
events {
    worker_connections 1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    # 로그 형식 설정
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                   '$status $body_bytes_sent "$http_referer" '
                   '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;
    error_log /var/log/nginx/error.log warn;

    # 성능 최적화
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    # Gzip 압축 설정
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_types
        text/plain
        text/css
        text/xml
        text/javascript
        application/javascript
        application/xml+rss
        application/json;

    server {
        listen 80;
        server_name localhost;
        root /usr/share/nginx/html;
        index index.html;

        # 보안 헤더 설정
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;
        add_header Referrer-Policy "strict-origin-when-cross-origin" always;

        # React Router 지원을 위한 설정
        location / {
            try_files $uri $uri/ /index.html;
        }

        # 정적 자산 캐시 설정
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }

        # API 프록시 설정
        location /api/ {
            proxy_pass http://backend:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        # 헬스 체크 엔드포인트
        location /health {
            access_log off;
            return 200 "healthy\n";
            add_header Content-Type text/plain;
        }
    }
}
```

### Docker Compose 구성

#### 1. 개발 환경 (docker-compose.dev.yml)
```yaml
version: '3.8'

services:
  # 데이터베이스
  postgres:
    image: postgres:15-alpine
    container_name: app-postgres-dev
    environment:
      POSTGRES_DB: appdb_dev
      POSTGRES_USER: app_dev
      POSTGRES_PASSWORD: dev_password
    ports:
      - "5432:5432"
    volumes:
      - postgres_dev_data:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - app-network

  # Redis
  redis:
    image: redis:7-alpine
    container_name: app-redis-dev
    ports:
      - "6379:6379"
    volumes:
      - redis_dev_data:/data
    networks:
      - app-network

  # 백엔드
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: app-backend-dev
    environment:
      SPRING_PROFILES_ACTIVE: dev
      DB_HOST: postgres
      DB_PORT: 5432
      DB_NAME: appdb_dev
      DB_USERNAME: app_dev
      DB_PASSWORD: dev_password
      REDIS_HOST: redis
      REDIS_PORT: 6379
    ports:
      - "8080:8080"
    depends_on:
      - postgres
      - redis
    volumes:
      - ./backend/logs:/app/logs
    networks:
      - app-network
    restart: unless-stopped

  # 프론트엔드
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      args:
        REACT_APP_API_BASE_URL: http://localhost:8080
        REACT_APP_ENV: development
    container_name: app-frontend-dev
    ports:
      - "3000:80"
    depends_on:
      - backend
    networks:
      - app-network
    restart: unless-stopped

volumes:
  postgres_dev_data:
  redis_dev_data:

networks:
  app-network:
    driver: bridge
```

#### 2. 프로덕션 환경 (docker-compose.prod.yml)
```yaml
version: '3.8'

services:
  # 데이터베이스
  postgres:
    image: postgres:15-alpine
    container_name: app-postgres-prod
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USERNAME}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_prod_data:/var/lib/postgresql/data
      - ./database/backup:/backup
    networks:
      - app-network
    restart: always
    deploy:
      resources:
        limits:
          memory: 1G
          cpus: '0.5'

  # Redis
  redis:
    image: redis:7-alpine
    container_name: app-redis-prod
    command: redis-server --requirepass ${REDIS_PASSWORD}
    environment:
      REDIS_PASSWORD: ${REDIS_PASSWORD}
    volumes:
      - redis_prod_data:/data
    networks:
      - app-network
    restart: always
    deploy:
      resources:
        limits:
          memory: 512M
          cpus: '0.25'

  # 백엔드
  backend:
    image: ${DOCKER_REGISTRY}/app-backend:${VERSION}
    container_name: app-backend-prod
    environment:
      SPRING_PROFILES_ACTIVE: prod
      DB_HOST: postgres
      DB_PORT: 5432
      DB_NAME: ${DB_NAME}
      DB_USERNAME: ${DB_USERNAME}
      DB_PASSWORD: ${DB_PASSWORD}
      REDIS_HOST: redis
      REDIS_PORT: 6379
      REDIS_PASSWORD: ${REDIS_PASSWORD}
    depends_on:
      - postgres
      - redis
    volumes:
      - ./logs:/app/logs
    networks:
      - app-network
    restart: always
    deploy:
      replicas: 2
      resources:
        limits:
          memory: 2G
          cpus: '1'

  # 프론트엔드
  frontend:
    image: ${DOCKER_REGISTRY}/app-frontend:${VERSION}
    container_name: app-frontend-prod
    depends_on:
      - backend
    networks:
      - app-network
    restart: always
    deploy:
      replicas: 2
      resources:
        limits:
          memory: 512M
          cpus: '0.5'

  # 로드 밸런서 (Nginx)
  nginx:
    image: nginx:alpine
    container_name: app-nginx-prod
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/ssl:/etc/nginx/ssl
      - ./logs/nginx:/var/log/nginx
    depends_on:
      - frontend
      - backend
    networks:
      - app-network
    restart: always

volumes:
  postgres_prod_data:
  redis_prod_data:

networks:
  app-network:
    driver: bridge
```

### CI/CD 파이프라인 구성

#### 1. GitHub Actions 워크플로우
```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  DOCKER_REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Run backend tests
        run: |
          cd backend
          ./gradlew test jacocoTestReport

      - name: Run frontend tests
        run: |
          cd frontend
          npm ci
          npm run test:ci

      - name: Upload coverage reports
        uses: codecov/codecov-action@v3
        with:
          files: backend/build/reports/jacoco/test/jacocoTestReport.xml,frontend/coverage/lcov.info

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    outputs:
      version: ${{ steps.version.outputs.version }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Generate version
        id: version
        run: |
          if [[ ${{ github.ref }} == 'refs/heads/main' ]]; then
            VERSION=$(date +%Y%m%d)-$(echo ${{ github.sha }} | cut -c1-7)
          else
            VERSION=$(date +%Y%m%d)-$(echo ${{ github.sha }} | cut -c1-7)-dev
          fi
          echo "version=$VERSION" >> $GITHUB_OUTPUT

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Docker Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.DOCKER_REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push backend image
        uses: docker/build-push-action@v5
        with:
          context: ./backend
          file: ./backend/Dockerfile
          push: true
          tags: |
            ${{ env.DOCKER_REGISTRY }}/${{ env.IMAGE_NAME }}-backend:${{ steps.version.outputs.version }}
            ${{ env.DOCKER_REGISTRY }}/${{ env.IMAGE_NAME }}-backend:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max

      - name: Build and push frontend image
        uses: docker/build-push-action@v5
        with:
          context: ./frontend
          file: ./frontend/Dockerfile
          push: true
          build-args: |
            REACT_APP_API_BASE_URL=${{ secrets.API_BASE_URL }}
            REACT_APP_ENV=production
          tags: |
            ${{ env.DOCKER_REGISTRY }}/${{ env.IMAGE_NAME }}-frontend:${{ steps.version.outputs.version }}
            ${{ env.DOCKER_REGISTRY }}/${{ env.IMAGE_NAME }}-frontend:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/develop'
    environment: staging

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to staging
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USER }}
          key: ${{ secrets.STAGING_SSH_KEY }}
          script: |
            cd /opt/app
            export VERSION=${{ needs.build.outputs.version }}
            export DOCKER_REGISTRY=${{ env.DOCKER_REGISTRY }}
            export IMAGE_NAME=${{ env.IMAGE_NAME }}
            
            # 환경 변수 로드
            source .env.staging
            
            # 새 이미지 풀
            docker-compose -f docker-compose.staging.yml pull
            
            # 무중단 배포
            docker-compose -f docker-compose.staging.yml up -d
            
            # 헬스 체크
            for i in {1..10}; do
              if curl -f http://localhost/health; then
                echo "Health check passed"
                break
              fi
              echo "Health check failed, retrying in 10s..."
              sleep 10
            done

  deploy-production:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to production
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USER }}
          key: ${{ secrets.PROD_SSH_KEY }}
          script: |
            cd /opt/app
            export VERSION=${{ needs.build.outputs.version }}
            export DOCKER_REGISTRY=${{ env.DOCKER_REGISTRY }}
            export IMAGE_NAME=${{ env.IMAGE_NAME }}
            
            # 환경 변수 로드
            source .env.production
            
            # 데이터베이스 백업
            docker exec app-postgres-prod pg_dump -U $DB_USERNAME $DB_NAME > backup-$(date +%Y%m%d-%H%M%S).sql
            
            # 새 이미지 풀
            docker-compose -f docker-compose.prod.yml pull
            
            # 블루-그린 배포
            docker-compose -f docker-compose.prod.yml up -d --scale backend=4
            sleep 30
            docker-compose -f docker-compose.prod.yml up -d --scale backend=2
            
            # 헬스 체크
            for i in {1..15}; do
              if curl -f https://app.company.com/health; then
                echo "Production health check passed"
                break
              fi
              echo "Health check failed, retrying in 15s..."
              sleep 15
            done
            
            # 오래된 이미지 정리
            docker image prune -f

  notify:
    needs: [deploy-staging, deploy-production]
    runs-on: ubuntu-latest
    if: always()

    steps:
      - name: Notify Slack
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          channel: '#deployments'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

### 배포 명령어 및 스크립트

#### 1. 로컬 개발 환경 실행
```bash
# 전체 스택 실행
docker-compose -f docker-compose.dev.yml up -d

# 로그 확인
docker-compose -f docker-compose.dev.yml logs -f

# 특정 서비스만 재시작
docker-compose -f docker-compose.dev.yml restart backend

# 개발 환경 종료
docker-compose -f docker-compose.dev.yml down
```

#### 2. 프로덕션 배포 스크립트
```bash
#!/bin/bash
# deploy.sh

set -e

# 환경 변수 설정
source .env.production

# 버전 정보
VERSION=${1:-$(date +%Y%m%d)-$(git rev-parse --short HEAD)}
echo "Deploying version: $VERSION"

# 이미지 빌드
echo "Building images..."
docker build -t $DOCKER_REGISTRY/app-backend:$VERSION ./backend
docker build -t $DOCKER_REGISTRY/app-frontend:$VERSION ./frontend

# 이미지 푸시
echo "Pushing images to registry..."
docker push $DOCKER_REGISTRY/app-backend:$VERSION
docker push $DOCKER_REGISTRY/app-frontend:$VERSION

# 배포
echo "Deploying to production..."
export VERSION=$VERSION
docker-compose -f docker-compose.prod.yml pull
docker-compose -f docker-compose.prod.yml up -d

# 헬스 체크
echo "Performing health check..."
for i in {1..10}; do
    if curl -f http://localhost/health; then
        echo "✅ Deployment successful!"
        exit 0
    fi
    echo "⏳ Waiting for application to start..."
    sleep 10
done

echo "❌ Deployment failed - health check timeout"
exit 1
```

#### 3. 롤백 스크립트
```bash
#!/bin/bash
# rollback.sh

set -e

PREVIOUS_VERSION=${1:-"latest"}

echo "Rolling back to version: $PREVIOUS_VERSION"

# 이전 버전으로 롤백
export VERSION=$PREVIOUS_VERSION
docker-compose -f docker-compose.prod.yml pull
docker-compose -f docker-compose.prod.yml up -d

# 헬스 체크
for i in {1..10}; do
    if curl -f http://localhost/health; then
        echo "✅ Rollback successful!"
        exit 0
    fi
    sleep 10
done

echo "❌ Rollback failed"
exit 1
```

### 모니터링 및 로깅

#### 1. 로그 수집 설정
```yaml
# docker-compose.logging.yml
version: '3.8'

services:
  # ELK Stack for 로그 수집
  elasticsearch:
    image: elasticsearch:8.8.0
    environment:
      - discovery.type=single-node
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - elasticsearch_data:/usr/share/elasticsearch/data

  logstash:
    image: logstash:8.8.0
    volumes:
      - ./logstash/pipeline:/usr/share/logstash/pipeline
    depends_on:
      - elasticsearch

  kibana:
    image: kibana:8.8.0
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

  # Prometheus for 메트릭 수집
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml

  # Grafana for 대시보드
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana

volumes:
  elasticsearch_data:
  grafana_data:
```

#### 2. 백업 및 복구 전략
```bash
#!/bin/bash
# backup.sh

# 데이터베이스 백업
docker exec app-postgres-prod pg_dump -U $DB_USERNAME $DB_NAME | gzip > "backup-$(date +%Y%m%d-%H%M%S).sql.gz"

# Redis 백업
docker exec app-redis-prod redis-cli BGSAVE

# 백업 파일 S3 업로드 (선택사항)
aws s3 cp backup-*.sql.gz s3://app-backups/database/

# 7일 이상된 백업 파일 삭제
find ./backup -name "backup-*.sql.gz" -mtime +7 -delete

echo "✅ Backup completed successfully"
```

### 보안 및 환경 변수 관리

#### 1. 환경 변수 파일 템플릿
```bash
# .env.production.template
# 데이터베이스 설정
DB_HOST=postgres
DB_PORT=5432
DB_NAME=appdb_prod
DB_USERNAME=app_prod
DB_PASSWORD=<strong_password>

# Redis 설정
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_PASSWORD=<redis_password>

# Docker Registry
DOCKER_REGISTRY=ghcr.io/company
VERSION=latest

# SSL 인증서 (Let's Encrypt)
SSL_EMAIL=admin@company.com
DOMAIN=app.company.com

# 모니터링
GRAFANA_ADMIN_PASSWORD=<grafana_password>

# JWT 시크릿
JWT_SECRET=<jwt_secret_key>

# 외부 API 키
MAIL_API_KEY=<sendgrid_api_key>
SLACK_WEBHOOK_URL=<slack_webhook>
```

#### 2. Docker Secrets 사용 (Docker Swarm)
```yaml
# docker-compose.swarm.yml
version: '3.8'

services:
  backend:
    image: app-backend:latest
    secrets:
      - db_password
      - jwt_secret
    environment:
      DB_PASSWORD_FILE: /run/secrets/db_password
      JWT_SECRET_FILE: /run/secrets/jwt_secret

secrets:
  db_password:
    external: true
  jwt_secret:
    external: true
```

### 성능 최적화 및 스케일링

#### 1. 수평 스케일링 설정
```bash
# 백엔드 서비스 스케일 업
docker-compose -f docker-compose.prod.yml up -d --scale backend=4

# 로드 밸런서 설정 확인
docker-compose -f docker-compose.prod.yml exec nginx nginx -t

# 서비스 상태 확인
docker-compose -f docker-compose.prod.yml ps
```

#### 2. 리소스 제한 및 모니터링
```yaml
# docker-compose.yml에서 리소스 제한
services:
  backend:
    deploy:
      resources:
        limits:
          memory: 2G
          cpus: '1.0'
        reservations:
          memory: 1G
          cpus: '0.5'
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
```

### 환경별 배포 전략

#### 1. 개발 환경 배포
```bash
# 개발 환경 시작
docker-compose -f docker-compose.dev.yml up -d

# 코드 변경 시 핫 리로드 (개발 중)
docker-compose -f docker-compose.dev.yml restart backend frontend

# 로그 모니터링
docker-compose -f docker-compose.dev.yml logs -f backend
```

#### 2. 스테이징 환경 배포
```bash
# 스테이징 배포 (자동화된 테스트 포함)
./scripts/deploy-staging.sh

# 통합 테스트 실행
docker-compose -f docker-compose.staging.yml exec backend ./gradlew integrationTest

# E2E 테스트 실행
docker-compose -f docker-compose.staging.yml exec frontend npm run test:e2e
```

#### 3. 프로덕션 환경 배포
```bash
# 프로덕션 배포 (승인 후)
./scripts/deploy-production.sh v1.2.3

# 헬스 체크 및 모니터링
./scripts/health-check.sh

# 트래픽 모니터링
docker-compose -f docker-compose.prod.yml exec nginx tail -f /var/log/nginx/access.log
```

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
