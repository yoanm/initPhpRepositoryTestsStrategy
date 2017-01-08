# PhpUnit implementation

> *PhpUnit implementation compliant to this* **[Tests strategy](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md)**

## Configuration reference
```xml
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:noNamespaceSchemaLocation="http://schema.phpunit.de/4.8/phpunit.xsd"
  stopOnError="true"
  stopOnFailure="true"
  convertErrorsToExceptions="true"
  convertNoticesToExceptions="true"
  convertWarningsToExceptions="true"
  beStrictAboutOutputDuringTests="true"
  beStrictAboutChangesToGlobalState="true"
  beStrictAboutTestsThatDoNotTestAnything="true"
  backupGlobals="true"
  backupStaticAttributes="false"
  forceCoversAnnotation="true"
  checkForUnintentionallyCoveredCode="true"
  bootstrap="vendor/autoload.php"
  colors="true"
>
  <listeners>
        <listener class="Yoanm\InitPhpRepositoryTestsStrategy\PhpUnit\TestsStrategyListener"/>
  </listeners>

  <testsuites>
      <testsuite name="technical">
          <directory>tests/Technical/Unit/*</directory>
          <!-- define (and so, launch) integration tests after unit tests => slower than unit tests -->
          <directory>tests/Technical/Integration/*</directory>
      </testsuite>
      <!-- defined (and so, launch) functional tests tests after technical tests => slower than technical tests -->
      <testsuite name="functional">
          <directory>tests/Functional/*</directory>
      </testsuite>
  </testsuites>

  <filter>
    <whitelist>
      <directory>src</directory>
    </whitelist>
  </filter>
</phpunit>
```
## Requirements

  * `beStrictAboutChangesToGlobalState="true"`requires `backupGlobals="true"` in order to work

## [Rules](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules) validated with [configuration reference](#configuration-reference)

### Mandatory

#### [Early stop](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-early-stop)

* `stopOnError="true"`
* `stopOnFailure="true"`

#### [Strict mode](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-strict-mode)

* [Exit status](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#exit-status) : PhpUnit command will return a failed status if a failed or on error test exist
* [Fails if](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-strict-mode-fails-if)
 
 * [Php errors](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-strict-mode-fails-if-php-errors)

    * `convertErrorsToExceptions="true"`
    * `convertNoticesToExceptions="true"`
    * `convertWarningsToExceptions="true"`
    
 * [Risky tests](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-strict-mode-fails-if-risky-tests) (requires [`listener`](#listener))

    * [No Output](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-output) with `beStrictAboutOutputDuringTests="true"`
    * [No globals manipulation](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-manipulate-globals) with `beStrictAboutChangesToGlobalState="true"`
    * [No test that test nothing](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-test-nothing) with `beStrictAboutTestsThatDoNotTestAnything="true"`

#### [Risky test](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests)

 * [No Output](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-output) with `beStrictAboutOutputDuringTests="true"` 
 * [No globals manipulation](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-manipulate-globals) with `beStrictAboutChangesToGlobalState="true"`
 * [No test that test nothing](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-test-nothing) with `beStrictAboutTestsThatDoNotTestAnything="true"`

#### [Test isolation](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-tests-isolation)
    
 * [Globals backup](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-tests-isolation-globals) with `backupGlobals="true"`
      
   *Required by `beStrictAboutChangesToGlobalState="true"`*

 * [Static class member backup](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-tests-isolation-static-class-member) with `backupStaticAttributes="false"`
  
#### [Real coverage](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-real-coverage)
    
 * [No coverage overflow](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-real-coverage-overflow)
      
  * `forceCoversAnnotation="true"`

  If no annotation, no coverage.

  Symply add the following as test class or test method comment

  ```
  /**
   * @covers FULLY\QUALIFIED\NAMESPACE\TO\MyClass
   */
  ```

 * [Risky tests does not count in coverage](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-real-coverage-risky-tests)
    
  * [No Output](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-output) with `beStrictAboutOutputDuringTests="true"` (requires [`listener`](#listener))
  * [No globals manipulation](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-manipulate-globals) with `beStrictAboutChangesToGlobalState="true"`
  * [No test that test nothing](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-test-nothing) with `beStrictAboutTestsThatDoNotTestAnything="true"`

<a name="listener"></a>
#### \<listener> (See [TestsStrategyListener](../src/PhpUnit/TestsStrategyListener.php))
      
Listener will validate following mandatory rules

 * [Strict mode - fails if - risky tests](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-strict-mode-fails-if-risky-tests)

  * Required by 
      
    * `beStrictAboutOutputDuringTests="true"` ([No Output](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-output))
    * `beStrictAboutChangesToGlobalState="true"` ([No globals manipulation](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-manipulate-globals))
    * `beStrictAboutTestsThatDoNotTestAnything="true"` ([No test that test nothing](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-test-nothing))

 * [Real coverage - risky tests  does not count in coverage](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-real-coverage-risky-tests) for some specific kinds of risky test   
      
  * Required by `beStrictAboutOutputDuringTests="true"` ([No Output](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-risky-tests-output))
 
<a name="test-suites"></a>
#### \<test-suites>
    
  * [Tests Root directory](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#tests-root-directory)
  * [Tests order](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#tests-order)

### Optional

 * [Test doc - tested class](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-test-documentation-tested-class-description) : by using `@covers`
      
   *In fact, required if coverage is used as configuration uses `forceCoversAnnotation="true"`*

 * [Test doc - tested class dependencies](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-test-documentation-tested-class-dependencies-description) by using `@uses`
  
    * Also use `checkForUnintentionallyCoveredCode="true"`, to be sure sure that new dependencies will be forced to be documented. With [configuration reference](#configuration-reference), it will convert test into risky test, and risky test will be converted into failed test by [`listener`](#listener)
      
    Simply add the following as test class or test method comment : 
    ```
    /**
     * @uses FULLY\QUALIFIED\NAMESPACE\TO\MyClassDependencyAClass
     * @uses FULLY\QUALIFIED\NAMESPACE\TO\MyClassDependencyBClass
     */
     class MyClassTest extends \PHPUnit_Framework_TestCase
     ```

## Optional config
  
 * `bootstrap="vendor/autoload.php"` : Autoload file
 * `colors="true"` : Pretty output
 * `processIsolation="true"` : For [test isolation - different process](https://github.com/yoanm/Readme/blob/master/TESTS_STRATEGY.md#rules-tests-isolation-different-process), but it could create edge cases
