在 IntelliJ IDEA 中设置 Java 语言版本为 20 但仍然遇到 `"不支持发行版本 5"` 的错误，可能是由于项目的编译配置或模块设置不一致所导致的。以下是一些可能的原因及解决方案：

### 1. 检查项目结构中的模块 SDK 和语言级别

- 依次进入 `File` -> `Project Structure...`（或使用快捷键 `Ctrl + Alt + Shift + S`）。
- 在 `Project` 选项卡中，确保 `Project SDK` 设置为 Java 20。
- 确保 `Project language level` 也设置为 20（`'Java 20 - Pattern Matching for switch (Second Preview)'` 或类似描述）。

- 然后，检查 `Modules` 选项卡：
  - 选择项目的模块（通常是 `src`）。
  - 确保 `Language level` 设置为 `Project default` 或明确设置为 Java 20。
  - 确保 `Module SDK` 设置为 Java 20。

### 2. 检查编译器设置

- 进入 `File` -> `Settings...` -> `Build, Execution, Deployment` -> `Compiler` -> `Java Compiler`。
- 在 `Per-module bytecode version` 中，确保所有模块的 `Target bytecode version` 设置为 `20`。

### 3. 检查 Maven 或 Gradle 配置

如果你使用的是 Maven 或 Gradle 构建工具，确保构建配置文件没有将 Java 版本指定为 5。

- **Maven** (`pom.xml` 文件)：
  - 确保 `maven-compiler-plugin` 的 `source` 和 `target` 属性都设置为 `20`。

  ```xml
  <build>
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.8.1</version>
              <configuration>
                  <source>20</source>
                  <target>20</target>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```

- **Gradle** (`build.gradle` 文件)：
  - 确保 `sourceCompatibility` 和 `targetCompatibility` 设置为 `20`。

  ```groovy
  sourceCompatibility = '20'
  targetCompatibility = '20'
  ```

### 4. 清除缓存和重新构建项目

有时 IntelliJ IDEA 的缓存可能导致问题。可以尝试以下步骤清除缓存并重新构建项目：

- 依次点击 `File` -> `Invalidate Caches / Restart...` -> `Invalidate and Restart`。
- 等待 IDEA 重新启动后，选择 `Build` -> `Rebuild Project` 重新构建项目。

### 5. 确认 IntelliJ IDEA 使用正确的 JDK 版本

确保 IntelliJ IDEA 使用的是你系统中安装的 Java 20 JDK，而不是其他版本的 JDK。

- 在 `File` -> `Project Structure...` -> `SDKs` 下，检查你添加的 JDK 路径，确保是 Java 20。

### 6. 检查项目和 IDE 的兼容性

确保你的 IntelliJ IDEA 版本支持 Java 20。某些旧版本的 IntelliJ IDEA 可能无法完全支持最新的 Java 语言版本。如果是这种情况，考虑更新 IntelliJ IDEA。

### 7. 升级和同步项目依赖

如果你使用的是 Maven 或 Gradle，确保所有依赖项、插件版本与 Java 20 兼容。

- 对于 Maven，运行 `mvn clean install`。
- 对于 Gradle，运行 `gradle clean build`。

### 总结

通过检查以上设置，通常可以解决 IntelliJ IDEA 中关于 `"不支持发行版本 5"` 的错误。如果仍然遇到问题，请提供更多详细信息，比如错误日志或项目设置截图，这样我可以更好地帮助你解决问题。