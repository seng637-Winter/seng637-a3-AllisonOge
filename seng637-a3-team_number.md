**SENG 637 - Dependability and Reliability of Software Systems**

**Lab. Report #3 – Code Coverage, Adequacy Criteria and Test Case Correlation**

| Group Number 19      |
| -------------- |
| Sami Abdelhalem |
| Mohammad Hallaq |
| Ogechukwu Kanu |
| Emmanuel Alafonye |

**Table of Content**

[1 Introduction](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#1-introduction)

[2 Manual data-flow coverage calculations for `DataUtilities.calculateColumnTotal` and `Range.expand` methods](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#2-manual-data-flow-coverage-calculations-for-datautilitiescalculatecolumntotal-and-rangeexpand-methods)

[3 A detailed description of the testing strategy for the new unit test](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#3-a-detailed-description-of-the-testing-strategy-for-the-new-unit-test)

[4 A high-level description of three selected test cases you have designed using coverage information, and how they have increased code coverage](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#4-a-high-level-description-of-three-selected-test-cases-you-have-designed-using-coverage-information-and-how-they-have-increased-code-coverage)

[5 A detailed report of the coverage achieved of each class and method](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#5-a-detailed-report-of-the-coverage-achieved-of-each-class-and-method)

[6 Pros and Cons of coverage tools used and Metrics you report](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#6-pros-and-cons-of-coverage-tools-used-and-metrics-you-report)

[7 A comparison of the advantages and disadvantages of requirements-based test generation and coverage-based test generation.](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#7-a-comparison-of-the-advantages-and-disadvantages-of-requirements-based-test-generation-and-coverage-based-test-generation)

[8 A discussion on how the teamwork/effort was divided and managed](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#8-a-discussion-on-how-the-teamworkeffort-was-divided-and-managed)

[9 Any difficulties encountered, challenges overcome, and lessons learned from performing the lab](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#9-any-difficulties-encountered-challenges-overcome-and-lessons-learned-from-performing-the-lab)

[10 Comments/feedback on the lab itself](https://github.com/seng637-Winter/seng637-a3-AllisonOge/blob/main/seng637-a3-team_number.md#10-commentsfeedback-on-the-lab-itself)

# 1 Introduction

In this lab report, we discuss the analysis of the developed test suite from the previous lab using the code coverage tool EclEmma and how the test suite is improved for better code coverage. This was done by first familiarizing ourselves with the assignment instructions and understanding the required tasks to be completed. 

In the following sections, we will manually compute the data-flow coverage for two methods, `DataUtilities.calculateColumnTotal` and `Range.expand`, devise a test strategy to improve the code coverage of the existing test suite, give a high-level overview of five of the test cases that were added, analyse the code coverage across the two classes (`DataUtilities` and `Range`) and their methods (all ten of them) with the improved test suite while stating any pros and cons observed. Then we compare the black box and white box testing techniques as employed in the labs, how the work was distributed among the team and the challenges and solutions used.

>The test suite can be found in the `org.jfree.data.test` package of the source directory.

# 2 Manual data-flow coverage calculations for DataUtilities.calculateColumnTotal and Range.expand methods

The data-flow coverage calculation for DataUtilities.calculateColumnTotal method is as follows. We first developed the data flow graph below and determined the Def-Use table to determine the data-flow coverage corresponding to that computed by the EclEmma library for the test cases developed during the black-box testing lab.
![DFG-calculateColumnTotal](media/DFG-calculateColumnTotal.png)
*Data Flow graph for the `DataUtilities.calculateColumnTotal` method*
![CodeCoverage-calculateColumnTotal](media/BB-calculateColumnTotal.png)
*`DataUtilities.calculateColumnTotal` method code*

From the DFG shown, we developed the Def-Use table as follows
| Variable (v) | Definition at node (n) | Use at node (n) |
| --- | --- | --- |
| data | 123 | 124, 126, 128, 134 |
| column | 123 | 128, 134 |
| total | 125 | 130, 136, 139 |
| total | 130 | 130, 136, 139 |
| total | 136 | 136, 139 |
| rowCount | 126 | 127, 133 |
| r* | 127 | 127 |
| r2* | 133 | 133 |
| n | 128 | 129, 130 |
| n | 134 | 135, 136 |

>*Associated with a for loop so it is multiplied by 4. See below for details
>```
>for ( a = 0;	// 1
>	a < 3;	// 2
>	a++ ) { } // 3
>DU pair set for a  is {(1,3), (1, 3), (3, 2), (3,3)} (4 pairs)
>```

The Def-Use sets for the variables are as follows.

For variable `data`: {(123, 124), (123, 126), (123, 128), (123, 134)} (4 pairs)<br/>
For variable `column`: {(123, 128), (123, 134)} (2 pairs)<br/>
For variable `total`: {(125, 130), (125, 136), (125, 139), (130, 130), (130, 136), (130, 139), (136, 136), (136, 139)} (8 pairs)<br/>
For variable `rowCount`: {(126, 127), (126, 133)} (2 pairs)<br/>
For variable `r`: {(127, 127)} (1 pair) * 4<br/>
For variable `r2`: {(133, 133)} (1 pair) * 4<br/>
For variable `n`: {(128, 129), (128, 130), (134, 135), (134, 136)} (4 pairs)

Thus, the total Def-Use pairs is 28 pairs.

The test cases developed from the black-box testing lab are summarized in the following decision table.
| Test Cases | Data Null | Column index | Expected Outcome |
| --- | --- | --- | --- |
| 1 | No | Valid (1) | The sum of the column (10.0) |
| 2 | No | Out-of-bounds (-1) | 0 |
| 3 | Yes | Any (0) | InvalidParameterException |

Considering each test case, we will determine the Def-Use pairs covered<br/>
**TC1**:
The test case sets the arguments of the method as follows:<br/>
>int column = 1<br/>
Values2D data = 2X2 (mock data)<br/>

The following Def-Use coverage is reached.

For variable `data`: {(123, 124), (123, 126), (123, 128)} (3 pairs)<br/>
For variable `column`: {(123, 128)} (1 pair)<br/>
For variable `total`: {(125, 130), (125, 139), (130, 130), (130, 139)} (4 pairs)<br/>
For variable `rowCount`: {(126, 127), (126, 133)} (2 pairs)<br/>
For variable `r`: {(127, 127)} (1 pair) * 4<br/>
For variable `r2`: {(133, 133)} (1 pair) * 4<br/>
For variable `n`: {(128, 129), (128, 130)} (2 pairs)

This gives a total DU-pair coverage of 20 pairs.

**TC2**:
The test case sets the arguments of the method as follows:<br/>
>int column = -1<br/>
Values2D data = 3X1 (mock data)

The following Def-Use coverage is reached.

For variable `data`: {(123, 124), (123, 126), (123, 128)} (3 pairs)<br/>
For variable `column`: {(123, 128)} (1 pair)<br/>
For variable `total`: {(125, 130), (125, 139), (130, 130), (130, 139)} (4 pairs)<br/>
For variable `rowCount`: {(126, 127), (126, 133)} (2 pairs)<br/>
For variable `r`: {(127, 127)} (1 pair) * 4<br/>
For variable `r2`: {(133, 133)} (1 pair) * 4<br/>
For variable `n`: {(128, 129), (128, 130)} (2 pairs)

This also gives a total DU-pair coverage of 20 pairs.

**TC3**:
The test case sets the arguments of the method as follows:<br/>
>int column = 0<br/>
Values2D data = null

The following Def-Use coverage is reached.

For variable `data`: {(123, 124)} (1 pair)

The DU-pair coverage for this test is 1 pair.

Considering all the test cases and the total coverage we have 12 pairs. We can now compute the data-flow coverage as **`total DU pairs covered / total DU pairs = 20/28 = 0.714 or 71.4%`**.

The data-flow coverage calculation for the `Range.expand` method is as follows. We take similar steps as above with the `DataUtilities.calculateColumnTotal` method to compute the data-flow coverage. Thus the developed DFG is as shown.
![DFG-expand](media/DFG-expand.png)
*Data Flow graph for the `Range.expand` method*
![CodeCoverage-expand](media/BB-expand.png)
*`Range.expand` method code*

From the DFG shown, we developed the Def-Use table as follows
| Variable (v) | Definition at node (n) | Use at node (n) |
| --- | --- | --- |
| range | 349 | 351, 352, 353, 354 |
| lowerMargin | 349 | 353 |
| upperMargin | 349 | 354 |
| length | 352 | 353, 354 |
| lower | 353 | 355, 356, 357, 359 |
| upper | 354 | 355, 356, 359 |
| upper | 357 | 359 |

The Def-Use sets for the variables are as follows.

For variable `range`: {(349, 351), (349, 352), (349, 353), (349, 354)} (4 pairs)<br/>
For variable `lowerMargin`: {(349, 353)} (1 pair)<br/>
For variable `upperMargin`: {(349, 354)} (1 pair)<br/>
For variable `length`: {(352, 353), (352, 354)} (2 pairs)<br/>
For variable `lower`: {(353, 355), (353, 356), (353, 357), (353, 359)} (4 pairs)<br/>
For variable `upper`: {(354, 355), (354, 356), (354, 359), (357, 359)} (4 pairs)<br/>

Thus, the total Def-Use pairs is 16 pairs.

The test cases developed from the black-box testing lab are summarized in the following decision table.
| Valid Range | Lower Margin | Upper Margin | Expected Outcome |
| --- | --- | --- | --- |
| Yes | Within 0 - 1 (0.25) | Within 0 - 1 (0.25) | Valid expansion |
| Yes (empty range) | Within 0 - 1 (0.2) | Within 0 - 1 (0.2) | Valid expansion |
| Yes | Beyond 0 - 1 (-0.25) | Beyond 0 - 1 (-0.25) | InvalidParameterException |
| No | Any (0.25) | Any (0.5) | InvalidParameterException |

Considering each test case, we will determine the Def-Use pairs covered<br/>
**TC1**:
The test case sets the arguments of the method as follows:
>Range range = Range(2, 6)<br/>
double lowerMargin = 0.25<br/>
double upperMargin = 0.5

The following Def-Use coverage is reached.

For variable `range`: {(349, 351), (349, 352), (349, 353), (349, 354)} (4 pairs)<br/>
For variable `lowerMargin`: {(349, 353)} (1 pair)<br/>
For variable `upperMargin`: {(349, 354)} (1 pair)<br/>
For variable `length`: {(352, 353), (352, 354)} (2 pairs)<br/>
For variable `lower`: {(353, 355), (353, 359)} (2 pairs)<br/>
For variable `upper`: {(354, 355), (354, 359)} (2 pairs)

This gives a total DU-pair coverage of 12 pairs.

**TC2**:
The test case sets the arguments of the method as follows:
>Range range = Range(2, 2)<br/>
double lowerMargin = 0.2<br/>
double upperMargin = 0.2

The following Def-Use coverage is reached.

For variable `range`: {(349, 351), (349, 352), (349, 353), (349, 354)} (4 pairs)<br/>
For variable `lowerMargin`: {(349, 353)} (1 pair)<br/>
For variable `upperMargin`: {(349, 354)} (1 pair)<br/>
For variable `length`: {(352, 353), (352, 354)} (2 pairs)<br/>
For variable `lower`: {(353, 355), (353, 359)} (2 pairs)<br/>
For variable `upper`: {(354, 355), (354, 359)} (2 pairs)

This gives a total DU-pair coverage of 12 pairs.

**TC3**:
The test case sets the arguments of the method as follows:
>Range range = Range(2, 6)<br/>
double lowerMargin = -0.25<br/>
double upperMargin = -0.25

The following Def-Use coverage is reached.

For variable `range`: {(349, 351), (349, 352), (349, 353), (349, 354)} (4 pairs)<br/>
For variable `lowerMargin`: {(349, 353)} (1 pair)<br/>
For variable `upperMargin`: {(349, 354)} (1 pair)<br/>
For variable `length`: {(352, 353), (352, 354)} (2 pairs)<br/>
For variable `lower`: {(353, 355), (353, 359)} (2 pairs)<br/>
For variable `upper`: {(354, 355), (354, 359)} (2 pairs)

This gives a total DU-pair coverage of 12 pairs.

**TC4**:
The test case sets the arguments of the method as follows:
>Range range = Range=null<br/>
double lowerMargin = 0.25<br/>
double upperMargin = 0.5

The following Def-Use coverage is reached.

For variable `range`: {(349, 351)} (1 pair)

This gives a total DU-pair coverage of 1 pair.

Considering all the test cases and the total coverage we have 12 pairs. We can now compute the data-flow coverage as **`total DU pairs covered / total DU pairs = 12/16 = 0.75 or 75%`**.

The manually computed data flow coverage using the DU pair method correlates with the results shown by EclEmma code coverage as shown below concentrating on the highlighted areas.
![CodeCoverage-manual-check](media/CodeCoverage-manual-check.png)
*Verification of the manual coverage test as seen with the EclEmma tool*

# 3 A detailed description of the testing strategy for the new unit test

Following the insights gained from the previous lab, where our test suite was primarily designed using black-box techniques, we now aim to refine our approach by incorporating white-box testing techniques. Our focus will be on enhancing the code coverage for selected methods within the `Range` and `DataUtilities` classes. This endeavour will employ statement, branch, and method coverage metrics as our guiding principles.

## Objectives
- To enhance the code coverage of the five methods of the `Range` class that were addressed in the preceding lab.
- To improve the code coverage of the following methods of the `DataUtilities` class which are `DataUtilities.calculateColumnTotal` `DataUtilities.calculateRowTotal` and `DataUtilities.getCummulativePercentages`

## Test Strategies
The following test strategies will be employed in this lab

### Statement coverage
**Goal**: Ensure every executable statement in the method’s body is executed at least once.<br/>
**Approach**: Craft test cases that traverse all paths, ensuring each line of code within the targeted methods is executed. This involves:
- Identifying loops, conditional statements, and unique code blocks within each method.
- Designing inputs that trigger each part of the method, including normal operation, boundary conditions and error cases.

### Branch coverage
**Goal**: Guarantee that each possible branch from each decision point is executed.<br/>
**Approach**: Extend beyond statement coverage by focusing on conditional statements (e.g., if-else, switch-case) within the methods to ensure all logical branches are tested. This includes:
- Testing each condition that leads to different outcomes or branches.
- Including test cases for exception handling paths, especially in methods known to throw exceptions under specific conditions.

### Method coverage
**Goal**: Verify that each method is called and executed in the testing process.<br/>
**Approach**: Specific attention will be paid to:
- Directly calling and testing each method to ensure its functionality is validated.
- Incorporating integration tests where necessary, especially for methods in `DataUtilities` that interact with datasets, to validate their correct behaviour in a broader context.

## Implementing the Strategy
- Test cases will be developed iteratively, starting with simpler cases to achieve statement coverage, and progressively incorporating more complex scenarios to achieve branch and method coverage.
- Utilizing tools such as EclEmma will aid in identifying coverage metrics and uncovering untested code segments.
- Continuous refinement of test cases will be undertaken based on feedback from coverage analysis, aiming for comprehensive coverage and uncovering potential edge cases or bugs.

By employing these strategies, we aim to enhance the test coverage and thereby ensure a thorough validation of the `Range` and `DataUtilities` classes.

# 4 A high-level description of three selected test cases you have designed using coverage information, and how they have increased code coverage

In this section, we give a high-level description of five selected additional test cases using the test strategy discussed in the last section to increase the code coverage.

## Test Case: `testExpandWithInvalidMargin`
**Objective**: To test the behaviour when the margins are negative<br/>
**The strategy used**: Determine the combination of values for the margins and range object that will cause the condition `lower > upper` to be true.<br/>
**Implementation**: A new test case was added to reach the desired condition.<br/>
**Result**: The test coverage is increased from 75% to 100%
(a) | (b) 
:---:|:---:
![BB-expand](media/BB-expand.png)|![WB-expand](media/WB-expand.png)

*Test coverage increases with the test case and covers the branch*

## Test Case: `testPositiveRangeAllowingZeroCrossing`
**Objective**: To validate the Range.shift method's ability to correctly handle zero crossing.<br/>
**The strategy used**: Target the specific functionality allowing a positive range to shift into negative territory.<br/>
**Implementation**: Introduced a new test, ensuring it exercises the condition where a range crosses zero.<br/>
**Result**: Test coverage improved from 58.6% to 100%, effectively covering previously untested logic paths related to zero crossing.
(a) | (b) 
:---:|:---:
![BB-shift](media/BB-shift.png)|![WB-shift](media/WB-shift.png)

*Test coverage of the `shift` improved to 100%*

## Test Case: `testCumulativePercentagesSingleNullValue`
**Objective**: To validate the DataUtilities.getCumulativePercentages method's ability to handle null values in data correctly.<br/>
**The strategy used**: Define mock data expectations to include null values.<br/>
**Implementation**: Introduced a new test, ensuring it exercises the condition where the value of a single data is null.<br/>
**Result**: Test (branch) coverage improves from 58.3% to 75.0% (as measured by the EclEmma tool).
(a) | (b) 
:---:|:---:
![BB-getCumulativePercentages](media/BB-getCummulativePercentages.png)|![WB-getCumulativePercentages](media/WB-getCummulativePercentages.png)

*Test coverage increases with the test case and covers more branches as indicated by the reduction of yellow lines*

Out of all the methods we tested in the previous lab only three were improved to a better coverage using the code coverage white-box techniques.

# 5 A detailed report of the coverage achieved of each class and method

The code coverage achieved for each class and their methods are summarized as follows with screenshots showing the coverage with the EclEmma tool.

**`Range.getLength` method**

The feasible test coverage as shown in the figure below that returns the difference between the upper and lower boundaries has been exhausted in the black box testing stage because the condition statement is never reached as it is addressed in the constructor block.
![BB-getLength](media/BB-getLength.png)

*Test coverage of the `getLength` method*

**`Range.getLowerBound` method**

The feasible test coverage as shown in the figure below that returns the lower boundary has been exhausted because the condition statement is never reached as it is addressed in the constructor block.
![BB-getLowerBound](media/BB-getLowerBound.png)

*Test coverage of the `getLowerBound` method*

**`Range.getUpperBound` method**

The feasible test coverage as shown in the figure below that returns the upper boundary has been exhausted because the condition statement is never reached as it is addressed in the constructor block.
![BB-getUpperBound](media/BB-getUpperBound.png)

*Test coverage of the `getUpperBound` method*

**`Range.expand` method**

The test coverage was improved to 100% code coverage with white box testing techniques discussed in the previous section. See the code coverage in the figure below.
![WB-expand](media/WB-expand.png)

*Test coverage of the `expand` method*

**`Range.shift` method**

The test coverage was improved to 100% code coverage with white box testing techniques discussed in the previous section. See the code coverage in the figure below.
![WB-shift](media/WB-shift.png)

*Test coverage of the `shift` method*

**`DataUtilities.calculateColumnTotal` method**

The feasible test coverage for the method has been exhausted in the test suite and it is shown in the figure below. All attempts to reach infeasible areas resulted in an infinite loop which has been discussed in detail in section 9.
![BB-calculateColumnTotal](media/BB-calculateColumnTotal.png)

*Test coverage of the `calculateColumnTotal` method*

**`DataUtilities.calculateRowTotal` method**

The feasible test coverage for the method has been exhausted in the test suite and it is shown in the figure below. All attempts to reach infeasible areas resulted in an infinite loop which has been discussed in detail in section 9.
![BB-calculateRowTotal](media/BB-calculateRowTotal.png)

*Test coverage of the `calculateRowTotal` method*

**`DataUtilities.getCumulativePercentages` method**

Beyond the coverage achieved from the test cases developed from the last lab, an additional test case improved the branch coverage to 75.0% and it has been discussed in the previous section. The final test coverage achieved in the feasible region is shown in the figure below. All attempts to reach infeasible areas resulted in an infinite loop which has been discussed in detail in section 9.
![BB-getCumulativePercentages](media/WB-getCummulativePercentages.png)

*Test coverage of the `getCumulativePercentages` method*

**`DataUtilities.createNumberArray` method**

The test coverage is 100% code coverage as shown in the figure below. This was achieved in the last lab.
![BB-createNumberArray](media/BB-createNumberArray.png)

*Test coverage of the `createNumberArray` method*

**`DataUtilities.createNumberArray2D` method**

The test coverage is 100% code coverage as shown in the figure below. This was achieved in the last lab.
![BB-createNumberArray2D](media/BB-createNumberArray2D.png)

*Test coverage of the `createNumberArray2D` method*

# 6 Pros and Cons of coverage tools used and Metrics you report

Statement, branch and method coverage metrics are different metrics used in code coverage analysis to assess the effectiveness of a testing suite. However, there are some notable pros and cons to each of these metrics and we will briefly discuss them.

## Statement Coverage Metric

### Pros:
- **Simplicity**: It is easy to understand and measure (it checks whether each line of code has been executed at least once).
- **Initial Baseline**: It provides a quick overview of the untested parts of a codebase, which can be helpful for initial assessments.
### Cons:
- **Limited Depth**: A high statement coverage doesn’t guarantee that all execution paths are tested, as it doesn’t consider conditional logic.
- **False Confidence**: It can give a false sense of security. This is because 100% statement coverage doesn’t mean the code is free from defects.

## Branch Coverage Metric

### Pros:
- **Conditional Logic**: It tests both the true and false paths of each conditional, leading to more thorough testing of control structures.
- **Bug Detection**: It is more effective at finding bugs than statement coverage because it considers different outcomes of decisions.
### Cons:
- **Complexity**: It is more complex to implement and analyze, as it requires more test cases to cover all possible branches.
- **Potential Overhead**: It can lead to a significant increase in the number of test cases, especially in complex applications with many conditionals.

## Method Coverage Metric

### Pros:
- **High-Level Overview**: It ensures that every method in the codebase is called, which can be a good indicator of untested functionalities.
- **Simplification**: It simplifies test reporting by focusing on methods as the unit of measurement.
### Cons:
- **Incomplete Testing**: A 100% method coverage does not translate to coverage of all lines or branches within it.
- **Missed Scenarios**: It doesn’t account for different usage scenarios and methods interactions which may lead to important behaviour being untested.


# 7 A comparison of the advantages and disadvantages of requirements-based test generation and coverage-based test generation.

To show a comprehensive comparison of the two test generations, we will discuss the reports that inform us about how much of the code has been executed (covered) during testing. The requirement-based testing focuses on whether the software meets the specified requirements and it typically involves creating test cases based on user requirements and specifications without necessarily considering the internal structure of the code. The advantage of such a method is that it ensures the software functionality aligns with user expectations and requirements but there is the potential disadvantage of overlooking parts of the code that do not map directly to explicit requirements. On the other hand, code coverage testing emphasizes how much of the code is exercised by the test suite, which can measured in different granularities, such as method coverage, statement coverage, branch coverage, etc.. The primary advantage of code coverage is that it can identify untested parts of the code, exposing potential risks in areas that have not been executed during testing. However, high coverage does not necessarily mean high quality or the fulfilment of user requirements. It is possible to have a high coverage percentage with important functional scenarios still untested.

![CodeCoverage-BB](media/CodeCoverage-BB.png)

*Test coverage with requirement-based test generation using black box techniques*
![CC-latest-Range](media/CC-latest-Range.png)
![CC-latest-DataUtilties](media/CC-latest-DataUtilties.png)

*Test coverage with coverage-based test generation using white-box techniques*

Based on the reports above, the code coverage for the classes `Range` and `DataUtilties` indicate how thoroughly the tests exercise the code. For example, in the `Range` class, certain methods like `shiftWithNoZeroCrossing` have high coverage, indicating thorough testing, whereas there are missing test cases for methods of the `Range` class, pointing to potential areas for additional testing. Consequently, these uncovered additional testing is treated with coverage-based testing to reach an acceptable tradeoff between requirement coverage and code coverage. The statement, branch and method coverage that was achieved at the end of the lab for both classes are summarized below.

![CC-Range](media/CC-Range.png)

*Code coverage for the `Range` class with EclEmma tool*
![CC-DataUtilites](media/CC-DataUtilites.png)

*Code coverage for the `DataUtilities` class with EclEmma tool*


# 8 A discussion on how the teamwork/effort was divided and managed

Teamwork again was highly encouraged in this lab mostly because it is a continuation of the previous lab. Every member of the team contributed to improving the code coverage of the test suite and creating the lab report. The first stage involved every member brainstorming through the errors with the new codebase when integrating the test suite from the previous lab and then assigning duties based on strengths and continuation of assignments from the previous lab. That is if a member worked on developing the test cases for a method and the code coverage was not 100% the member had the duty of improving the code coverage to the maximum possible if 100% was not feasible.

Table 1: task assignments for each of the team members
| Team Member | Methods Under Test | Additional Duties |
| --- | --- | --- |
| Sami Abdelhalem | `org.jfree.data.test.ShiftTest` <br/> `org.jfree.data.test.GetCumulativePercentagesTest` | 
| Mohammad Hallaq | `org.jfree.data.test.GetUpperBoundTest` <br/>  `org.jfree.data.test.GetLowerBoundTest` <br/> `org.jfree.data.test.CreateNumberArrayTest` | |
| Ogechukwu Kanu | `org.jfree.data.test.ExpandTest` <br/> `org.jfree.data.DataUtilities.calculateRowTotalTest` <br/> `org.jfree.data.test.calculateColumnTotalTest` | Manual test coverage calculation, compilation of final code and finalizing lab report |
| Emmanuel Alafonye | `org.jfree.data.test.GetLengthTest`| |


# 9 Any difficulties encountered, challenges overcome, and lessons learned from performing the lab

With the codebase associated with this lab when testing our test cases, we encountered an issue with the test. The results we had from the previous lab were not reproducible with the latest codebase. We had no errors with our previous lab but when we tested it against the new codebase we encountered errors. The errors were mostly a result of the mock library used and its dependency on the `hamcrest` library. We fixed the issue by using the older version (1.1) instead of the version (1.3) that was associated with the codebase for this lab.

Another challenge that was managed is the attempts to improve the code coverage of the `DataUtilites.calculateColumnTotal` and `DataUtilities.calculateRowTotal` methods. The infeasible regions of the code led to an infinite loop when testing which had to be forcefully exited. A sample test case tested on the `DataUtilities.calculateColumnTotal` method to illustrate the process is shown below. The test case simulates an invalid `rowCount` of -2 using mock data so that the second for loop in the `DataUtilities.calculateColumnTotal` method runs but as expected the loop is never exited. This is shown by the highlights in the figure showing the test is stopped at 5 out of 43 test cases, the 5th test case being the `testInvalidData`.
![WB-calucateColumnTotal-TC](media/WB-calculateColumnTotal-TC.png)
*Test case attempting to test the infeasible region in the `DataUtilities.calculateColumnTotal` method*

The resulting test coverage with this attempt also confirms that the second loop is run as shown below.
![WB-calculateColumnTotal](media/WB-calculateColumnTotal.png)

*Code coverage for the test case*

The most important lesson learned from the lab was that it was effective to develop relevant testing cases through collaboration and sharing of ideas.

# 10 Comments/feedback on the lab itself

The lab helped to build teamwork, collaboration and sharing. We understood our strengths and weaknesses which enabled us to plan accordingly. We understood the data-flow coverage test and how it is computed with a code coverage tool like EclEmma as well as white box testing techniques. In conclusion, the lab was a success and we completed it successfully.
