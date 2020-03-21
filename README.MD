### How did we compile mutataion branch 

In force/hard coded change in mutation change, the test method comprises of a commented code.
```java
    @Test
    public void testWrongSequenceInNonStaticImports() throws Exception {
        final DefaultConfiguration checkConfig =
            createModuleConfig(ImportOrderCheck.class);
        checkConfig.addAttribute("useContainerOrderingForStatic", "false");
        checkConfig.addAttribute("sortStaticImportsAlphabetically", "false");
        checkConfig.addAttribute("option", "bottom");

        final String[] expected = {
           //"4:1: " + getCheckMessage(MSG_ORDERING,"java.util.HashMap"),
        };

        verify(checkConfig,
                getNonCompilablePath("InputImportOrderNonStaticWrongSequence.java"),
                expected);
    }
```

Now if we un comment b'cos of hard code this is going to fail.

Also, due to hard code for true as shown
```java
private boolean isWrongOrder(String name, boolean isStatic) {
        final boolean result;
        if (true) {
            if (useContainerOrderingForStatic) {
                result = compareContainerOrder(lastImport, name, caseSensitive) > 0;
            }
            else if (staticImportsApart) {
                result = sortStaticImportsAlphabetically
                    && compare(lastImport, name, caseSensitive) > 0;
            }
            else {
                result = compare(lastImport, name, caseSensitive) > 0;
            }
        }
        else {
            // out of lexicographic order
            result = compare(lastImport, name, caseSensitive) > 0;
        }
        return result;
    }
```
the PMD shows up two violations and thus the mvn command used to relax the same used as follows
```mvn -Dpmd.maxAllowedViolations=2 clean verify```
