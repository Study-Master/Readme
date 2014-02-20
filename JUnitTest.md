JUnit Test简单说明
===

Unit Test是为了测试每一个method都在正常工作，从理论上来说，一个Class需要对应一个TestClass，并且对其中的每一个method进行测试。测试过程中，需要举例不同的Test Case，确保测试覆盖了各类情况。

Java使用JUnit插件进行Unit Test。在使用Maven的项目中，默认已经引用了JUnit插件。所有的测试程序都被放在了test文件夹下。

##创建一个TestClass

进入test文件夹，可以发现该文件夹和main文件夹的layout相同。为了包管理方便，如果在Main中更改了文件夹的Layout，test文件夹也同样更改为一致的Layout。我们现在假设main中有一个App.java程序，packae为app。那么在test文件夹下，和main一样的目录下，创建AppTest.java。一般都是采取ClassTest.java的命名方式。同样package应该和App一致：

```Java
//App.java

package app;

public class App {

	public App(){}

	public int intMethod() { return 1; }

	public boolean booleanMethod() { return true; }

	public String stringMethod() { return "Hello World!" }
}
```

下面是一个AppTest.java程序：

```Java
//AppTest.java

package app;

import junit.framework.Test;
import junit.framework.TestCase;
import junit.framework.TestSuite;

public class AppTest extends TestCase{
	
	/**
     * Create the test case
     * @param  testName name of the test case
     */
    public AppTest(String testName) {
        super(testName);
    }

    /**
     * @return the suite of tests being tested
     */
    public static Test suite() {
        return new TestSuite(AppTest.class);
    }

    /**
     * Test intMethod
     */
    public void testIntMethod() {
    	App a = new App();
    	assertEquals(1, a.intMethod());
    }

    /**
     * Test booleanMethod
     */
    public void testBooleanMethod() {
    	App a = new App();
    	assertTrue(a.booleanMethod());
    }

    /**
     * Test stringMethod
     */
    public void testStringMethod() {
    	App a = new App();
    	String test = a.stringMethod();
    	if(!a.equals("This is not right!"))
    		assertTrue(true);
    	if(a.equals("Hello World!"))
    		assertTrue(true);
    }
}
```

由上图可知，一般测试用method都用test开头，后面跟随被测试的method。在测试一个method的时候，输入一个特定的参数，测试是否得到了一个特殊的解。一般来说，这些特定的参数具有随机性和代表性，能够涵盖基本所有的测试情况。