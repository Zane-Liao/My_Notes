集成测试和单元测试都是软件测试中的重要部分，但它们关注的范围和目的有所不同。以下是它们的区别：

### 单元测试（Unit Testing）

1. **范围**：
   - 单元测试的范围是最小的，通常测试单个函数、方法或类。其目的是验证每个独立单元的正确性。
   
2. **目的**：
   - 确保单个单元的逻辑正确性，捕获早期阶段的错误和缺陷。
   
3. **依赖性**：
   - 单元测试一般是独立的，不依赖其他单元。通过使用模拟对象（mock objects）和桩（stubs），可以隔离测试单元。
   
4. **编写者**：
   - 通常由开发人员在开发过程中编写和执行。
   
5. **速度**：
   - 执行速度非常快，因为它们只测试单个单元，不涉及复杂的依赖关系或外部系统。
   
6. **示例**：
   ```java
   public class Calculator {
       public int add(int a, int b) {
           return a + b;
       }
   }
   
   public class CalculatorTest {
       @Test
       public void testAdd() {
           Calculator calculator = new Calculator();
           assertEquals(5, calculator.add(2, 3));
       }
   }
   ```

### 集成测试（Integration Testing）

1. **范围**：
   - 集成测试的范围较大，通常测试多个单元、模块或系统之间的交互和集成情况。
   
2. **目的**：
   - 确保各个单元和模块能够正确地协同工作，验证接口、通信协议和数据传递的正确性。
   
3. **依赖性**：
   - 集成测试需要测试多个单元的协作，因此依赖于这些单元的实际实现，可能涉及数据库、网络、文件系统等外部资源。
   
4. **编写者**：
   - 通常由测试人员或开发人员编写，可能在开发的后期或独立的测试阶段进行。
   
5. **速度**：
   - 执行速度相对较慢，因为它们需要设置和处理多个单元、模块之间的交互，可能还需要访问外部资源。
   
6. **示例**：
   ```java
   public class UserService {
       private UserRepository userRepository;
       private EmailService emailService;
   
       public UserService(UserRepository userRepository, EmailService emailService) {
           this.userRepository = userRepository;
           this.emailService = emailService;
       }
   
       public void registerUser(User user) {
           userRepository.save(user);
           emailService.sendWelcomeEmail(user.getEmail());
       }
   }
   
   public class UserServiceIntegrationTest {
       @Test
       public void testRegisterUser() {
           UserRepository userRepository = new UserRepository();
           EmailService emailService = new EmailService();
           UserService userService = new UserService(userRepository, emailService);
   
           User user = new User("test@example.com");
           userService.registerUser(user);
   
           assertTrue(userRepository.exists(user));
           assertTrue(emailService.wasEmailSent(user.getEmail()));
       }
   }
   ```

### 主要区别总结

1. **测试粒度**：
   - 单元测试：针对单个功能单元（函数、方法、类）。
   - 集成测试：针对多个功能单元之间的集成和交互。

2. **测试目的**：
   - 单元测试：验证单个单元的正确性。
   - 集成测试：验证单元之间的协作和集成。

3. **依赖关系**：
   - 单元测试：独立，不依赖其他单元，使用模拟对象。
   - 集成测试：依赖多个单元的实际实现，可能涉及外部资源。

4. **执行速度**：
   - 单元测试：执行速度快。
   - 集成测试：执行速度相对较慢。

5. **编写时机**：
   - 单元测试：开发过程中编写。
   - 集成测试：开发后期或独立测试阶段编写。

这两种测试方法相辅相成，共同保证软件的高质量和可靠性。单元测试确保每个单元的正确性，而集成测试确保这些单元能正确协作。