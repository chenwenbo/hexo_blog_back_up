title: Junit_API记录
date: 2016-07-13 09:53:09
tags:
---

### 注释
* @Test 这个注释说明依附在 JUnit 的 public void 方法可以作为一个测试案例。
* @Before 有些测试在运行前需要创造几个相似的对象。在 public void 方法加该注释是因为该方法需要在 test 方法前运行。
* @After 如果你将外部资源在 Before 方法中分配，那么你需要在测试运行后释放他们。在 public void 方法加该注释是因为该方法需要在 test 方法后运行。
* @BeforeClass 在 public void 方法加该注释是因为该方法需要在类中所有方法前运行。
* @AfterClass 它将会使方法在所有测试结束后执行。这个可以用来进行清理活动。
* @Ignore 这个注释是用来忽略有关不需要执行的测试的。


### API
* void assertEquals(boolean expected, boolean actual) 检查两个变量或者等式是否平衡
* void assertFalse(boolean condition) 检查条件是假的
* void assertNotNull(Object object) 检查对象不是空的
* void assertNull(Object object) 检查对象是空的
* void assertTrue(boolean condition) 检查条件为真
* void fail() 在没有报告的情况下使测试不通过

* void assertEquals(boolean expected, boolean actual) 检查两个变量或者等式是否平衡
* void assertTrue(boolean expected, boolean actual) 检查条件为真
* void assertFalse(boolean condition) 检查条件为假
* void assertNotNull(Object object) 检查对象不为空
* void assertNull(Object object) 检查对象为空
* void assertSame(boolean condition) assertSame() 方法检查两个相关对象是否指向同一个对象
* void assertNotSame(boolean condition) assertNotSame() 方法检查两个相关对象是否不指向同一个对象
* void assertArrayEquals(expectedArray, resultArray) assertArrayEquals() 方法检查两个数组是否相等

### assertThat
Before you start implementing your own Matcher’s, you should look at the core matchers that come with JUnit already. Here is a list of the matcher methods:

**Core**

* any() Matches any object passed to it.
* is() A matcher that checks if the given objects are equal.
* describedAs() adds a description to the matcher

**Logical**

* allOf() Takes an array of matchers and must all match the expected object.
* anyOf() Takes an array of matcher and must match at least one of the matchers must report that it matches the target object.
* not() Check if the object negates what was passed.

**Object**

* equalTo() Equality check.
* instanceOf() Check if an object is an instance of a given/expected object.
* notNullValue() Check if the passed value is not null
* nullValue() Tests whether the given object is null or not null.
* sameInstance() Tests if the given object is the exact same instance as another.

The list above can all be used on assertThat method. It gives a wide range of possible scenarios to fully regress the extend of your algorithm, logic or processes of your application.

### demo
```
package com.areyes1.junitassertrue.sample;
 
import static org.junit.Assert.*;
import static org.hamcrest.CoreMatchers.*;
import org.junit.Before;
import org.junit.Test;
 
public class JUnitTestAssertThatAssertions {
     
    int totalNumberOfApplicants = 0;
    
    @Before
    public void setData(){
        this.totalNumberOfApplicants = 9;
    }
     
    @Test
    public void testAssertThatEqual() {
        assertThat("123",is("123"));
    }

    @Test
    public void testAssertThatNotEqual() {
        assertThat(totalNumberOfApplicants,is(123));
    }

    @Test
    public void testAssertThatObject() {
        assertThat("123",isA(String.class));
    }
     
    @Test
  	public void testAssertThatWMessage(){
        assertThat("They are not equal!","123",is("1234"));
    }
}
```

**Custome Matchers**
```
package com.areyes1.junitassertthat.sample;
 
import org.hamcrest.BaseMatcher;
import org.hamcrest.Description;
import org.hamcrest.Matcher;
 
public class CustomMatcher {
    public static Matcher matches(final Object expected){
            return new BaseMatcher() {
                protected Object expectedObject = expected;
                public boolean matches(Object item) {
                    return expectedObject.equals(item);
                }
                public void describeTo(Description description) {
                    description.appendText(expectedObject.toString());
                }
            };
    }
}

We can then use that matcher as part of our Junit Test source.
JUnitTestAssertThatCustomMatcher.java
view sourceprint?

package com.areyes1.junitassertthat.sample;

 
import static org.junit.Assert.*;
import static com.areyes1.junitassertthat.sample.CustomMatcher.*;
import static org.hamcrest.CoreMatchers.*;
 
import java.util.ArrayList;
import java.util.List;
 
import org.junit.Before;
import org.junit.Test;
 
public class JUnitTestAssertThatCustomMatcher {
 
    ArrayList listOfValidStrings = new ArrayList();
    private String inputValue = new String("Hello");
    @Before
    public void setData(){
        listOfValidStrings.add("object_1");
        listOfValidStrings.add("object_2");
        listOfValidStrings.add("object_3");
    }
     
    @Test
    public void testLogic(){
        assertThat(inputValue,matches("Hello"));
    }
}
```
