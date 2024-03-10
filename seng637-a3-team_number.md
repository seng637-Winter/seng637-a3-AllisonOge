**SENG 637 - Dependability and Reliability of Software Systems**

**Lab. Report #3 – Code Coverage, Adequacy Criteria and Test Case Correlation**

| Group Number 19      |
| -------------- |
| Sami Abdelhalem |
| Mohammad Hallaq |
| Ogechukwu Kanu |
| Emmanuel Alafonye |


(Note that some labs require individual reports while others require one report
for each group. Please see each lab document for details.)

# 1 Introduction

In this lab report, we discuss the 

# 2 Manual data-flow coverage calculations for DataUtilities.calculateColumnTotal and Range.expand methods

The data-flow coverage calculation for DataUtilities.calculateColumnTotal method is as follows. We first developed the data flow graph below and determined the Def-Use table to determine the data-flow coverage corresponding to that computed by the EclEmma library for the test cases developed during the black-box testing lab.
![DFG-calculateColumnTotal](https://github.com/seng637-Winter/seng637-a3-AllisonOge/assets/44110875/9b08f7bb-c0d9-40b8-853d-56d7a4e10f56)
*Data Flow graph for the `DataUtilities.calculateColumnTotal` method*
![CodeCoverage-calculateColumnTotal](https://github.com/seng637-Winter/seng637-a3-AllisonOge/assets/44110875/05dddcf0-ca9d-41dc-892a-989c5e4f99f0)
*`DataUtilities.calculateColumnTotal` method code*

From the DFG shown, we developed the Def-Use table as follows
| Variable (v) | Definition at node (n) | Use at node (n) |
| --- | --- | --- |
| data | 123 | 124, 126, 128, 134 |
| column | 123 | 128, 134 |
| total | 125 | 130, 136, 139 |
| rowCount | 126 | 127, 133 |
| r | 127 | 127 |
| r2 | 133 | 133 |
| n | 128 | 129, 130 |
| n | 134 | 135, 136 |

The Def-Use sets for the variables are as follows.

For variable `data`: {(123, 124), (123, 126), (123, 128), (123, 134)} (4 pairs)<br/>
For variable `column`: {(123, 128), (123, 134)} (2 pairs)<br/>
For variable `total`: {(125, 130), (125, 136), (125, 139)} (3 pairs)<br/>
For variable `rowCount`: {(126, 127), (126, 133)} (2 pairs)<br/>
For variable `r`: {(127, 127)} (1 pair)<br/>
For variable `r2`: {(133, 133)} (1 pair)<br/>
For variable `n`: {(128, 129), (128, 130), (134, 135), (134, 136)} (4 pairs)

Thus, the total Def-Use pairs is 17 pairs.

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
For variable `total`: {(125, 130), (125, 139)} (2 pairs)<br/>
For variable `rowCount`: {(126, 127), (126, 133)} (2 pairs)<br/>
For variable `r`: {(127, 127)} (1 pair)<br/>
For variable `r2`: {(133, 133)} (1 pair)<br/>
For variable `n`: {(128, 129), (128, 130)} (2 pairs)

This gives a total DU-pair coverage of 12 pairs.

**TC2**:
The test case sets the arguments of the method as follows:<br/>
>int column = -1<br/>
Values2D data = 3X1 (mock data)

The following Def-Use coverage is reached.

For variable `data`: {(123, 124), (123, 126), (123, 128)} (3 pairs)<br/>
For variable `column`: {(123, 128)} (1 pair)<br/>
For variable `total`: {(125, 130), (125, 139)} (2 pairs)<br/>
For variable `rowCount`: {(126, 127), (126, 133)} (2 pairs)<br/>
For variable `r`: {(127, 127)} (1 pair)<br/>
For variable `r2`: {(133, 133)} (1 pair)<br/>
For variable `n`: {(128, 129), (128, 130)} (2 pairs)<br/>

This also gives a total DU-pair coverage of 12 pairs.

**TC3**:
The test case sets the arguments of the method as follows:<br/>
>int column = 0<br/>
Values2D data = null

The following Def-Use coverage is reached.

For variable `data`: {(123, 124)} (1 pair)

The DU-pair coverage for this test is 1 pair.

Considering all the test cases and the total coverage we have 12 pairs. We can now compute the data-flow coverage as **`total DU pairs covered / total DU pairs = 12/17 = 0.706 or 70.6%`**.

The data-flow coverage calculation for the `Range.expand` method is as follows. We take similar steps as above with the `DataUtilities.calculateColumnTotal` method to compute the data-flow coverage. Thus the developed DFG is as shown.
![DFG-expand](https://github.com/seng637-Winter/seng637-a3-AllisonOge/assets/44110875/deed12f1-44f4-48fe-9ab6-a5b3a5a232e5)
*Data Flow graph for the `Range.expand` method*
![CodeCoverage-expand](https://github.com/seng637-Winter/seng637-a3-AllisonOge/assets/44110875/5d90517e-336e-4171-8871-5904581b328e)
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
![coverage-manual-check](https://github.com/seng637-Winter/seng637-a3-AllisonOge/assets/44110875/4ccea399-3b6f-4579-898e-9b6bc9ea26d9)
*Verification of the manual coverage test as seen with the EclEmma tool*

# 3 A detailed description of the testing strategy for the new unit test

Text…

# 4 A high-level description of five selected test cases you have designed using coverage information, and how they have increased code coverage

Text…

# 5 A detailed report of the coverage achieved of each class and method (a screenshot from the code cover results in green and red colour would suffice)

Text…

# 6 Pros and Cons of coverage tools used and Metrics you report

Text…

# 7 A comparison of the advantages and disadvantages of requirements-based test generation and coverage-based test generation.

![coverage-test-BB](https://github.com/seng637-Winter/seng637-a3-AllisonOge/assets/44110875/e4cc48d5-019c-429c-9177-19fab131aa82)
*Test coverage with requirement-based test generation using black box techniques*


# 8 A discussion on how the teamwork/effort was divided and managed

Text…

# 9 Any difficulties encountered, challenges overcome, and lessons learned from performing the lab

With the codebase associated with this lab when testing our test cases, we encountered an issue with the test. The results we had from the previous lab didn’t correlate w

# 10 Comments/feedback on the lab itself

Text…


