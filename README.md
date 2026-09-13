# BDD calculator laboratory

This Maven project implements the PDF's addition exercise and the assigned multiplication, division and power exercise using Java, Cucumber and JUnit. The PDF's old Cucumber imports have been updated to `io.cucumber`; the Given/When/Then workflow stays the same.

## Run in IntelliJ IDEA

1. Open this folder's `pom.xml` as a Maven project.
2. Set the project SDK and Maven runner JRE to **JDK 17 or newer**.
3. Reload Maven and allow dependency downloads (internet is needed on the first run).
4. Open `src/test/java/calculator/RunnerTest.java` and run the class using its green arrow. Alternatively, run `test` in the Maven tool window.
5. Check the green scenario results. Open `target/cucumber-report.html` in a browser for the generated report.

The Cucumber for Java and Gherkin IDE plugins are useful for editing feature files, but running `RunnerTest` does not require them.

## Run in a terminal

With JDK 17+ and Maven installed, from this folder:

```sh
mvn clean test
```

If using the included Maven Wrapper, only the JDK is needed:

```powershell
.\mvnw.cmd clean test
```

On macOS/Linux: `sh mvnw clean test`.

Run only the original PDF outline:

```powershell
.\mvnw.cmd test "-Dcucumber.filter.tags=@pdf_outline"
```

## What to read

| File | Purpose |
| --- | --- |
| `src/test/resources/features/addition.feature` | Original addition scenario and all three PDF outline rows, plus signed-result regression cases |
| `src/test/resources/features/calculator.feature` | Ordinary scenarios and outlines for multiplication, division, powers and errors |
| `src/test/java/calculator/MyStepdefs.java` | Maps Gherkin steps to calls and assertions; fixes the minus-sign regex |
| `src/main/java/calculator/Calculator.java` | Actual arithmetic, including power implemented with multiplication |
| `src/test/java/calculator/RunnerTest.java` | Cucumber/JUnit runner with explicit feature and glue locations |
| `REPORT-fa.md` | Short Persian explanation for part 2 and arithmetic assumptions |
| `evidence/` | Saved output from the actual verification runs |

`Given` stores input, `When` calls the calculator, and `Then` asserts the result or expected error. Each Examples row is a separate scenario execution. An unexpected exception fails a result scenario; an error scenario passes only if the exception type and message match.

## Arithmetic contract

The assignment specifies integer inputs but leaves fractional outputs and exceptional cases unspecified. This implementation uses signed 32-bit integer results:

- Non-exact division truncates toward zero: `7 / 2 = 3`, `-7 / 2 = -3`.
- Negative exponents are rejected, including bases 0 and 1.
- Exponent zero returns 1, including `0^0` as an explicit project convention.
- Power uses multiplication by squaring, so large exponents for bases 0, 1 and -1 finish quickly. It does not use `Math.pow`.
- Overflow, division by zero and unsupported operators raise explicit errors.

Tests cover the provided examples, sign combinations, zero operands, identity operands, odd/even/zero exponents, truncation, integer boundaries and the error categories above. This is representative partition and boundary coverage, not exhaustive enumeration of every integer pair.

## Laboratory deliverables

The repository contains the complete runnable exercise, the short Persian report
in `REPORT-fa.md`, and actual test results in `evidence/`. The verified run passed
all 69 scenarios. Clone or download this repository, then follow the run
instructions above. The original regex failure was reproduced in an isolated
copy; the committed source includes the fix.

## Reference documentation

- [Cucumber JVM installation](https://cucumber.io/docs/installation/java/)
- [Maven Surefire 3.5.4](https://maven.apache.org/surefire-archives/surefire-3.5.4/maven-surefire-plugin/plugin-info.html)

