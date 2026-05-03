# Diagnostic Report

**Report: Title** Delay in rotate_joint for RBA- 2201  
**Name** Sean Allen – Robotics and Controls Engineer Intern  
**Date** 5/1/  
**Ticket ID** #2437

---

## Executive Summary

The report was done to diagnose and confirm the reported delay in the rotate_joint  
command for model number- 2201 by gathering response time data and analyzing code.  

Investigation showed a 50% increase over the expected time for the rotate_joint  
command along with syntax errors and inefficient time complexity.

---

## Issue Description

- **Problem Statement:** Outline the primary issue observed as stated in the ticket  
  There was a delayed response in the surgical robotic arm’s rotation function, impacting  
  real-time adjustments which could impact surgical activity. The ticket reports slower-than-  
  expected response times in the rotate_joint command which narrows the issue down.

- **Symptoms and Impact:** Describe the effects of the delay on surgical performance and potential risks.  
  Slow response times can increase the chances of critical risks during surgical procedures.  
  It can also distract any employees while lowering the precision of both the employee and device.

- **Client Information:**
  - **Hospital Name:** Mercy General Hospital  
  - **Location:** Rockville, Maryland  

- **Reported By:** Dr. Emily Chen, Senior Surgeon

---

## Diagnostic Process

- **Initial Observations:**  
  The move_arm function’s response time almost matches the expected time at.  
  while the other two differ. Rotate_joint was at .15 instead of the expected .1 while  
  adjust_grip was considerably under at .05 instead of the expected .9.

- **Commands Tested:**  
  The first tested command is check_response_time because this is where the initial  
  response time testing is done. Then the optimized_command was testing because  
  this is where changes were made for improving response times.

- **Hypothesis:** Provide initial guesses on causes of delays.  
  There’s a high change the problem is associated with the improvement_factor  
  variable in optimized_command where inaccurate optimizations are being made.  
  There is also the possibility of code inefficiencies as unnecessary repeated calls  
  were made to multiple functions.

- **Tools and Techniques Used:**  
  Iterative testing was applied to each cell for analyzing the control code through Python.  
  The control system diagnostic notebook was used for separating parts of the program into  
  testable pieces while Python helped to measure the response time.

---

## Findings and Analysis

**Response Time Data:**

| Command--| Expected Response Time-- | Initial Response Time--| Optimized Response Time |
|---------------|------------------------|------------------------|--------------------------|
| move_arm  | 0.10 seconds           | 0.1001 seconds         | 0.1001 seconds           |
| rotate_joint | 0.10 seconds           | 0.15 seconds           | 0.9 seconds              |
| adjust_grip  |0.09 seconds           | 0.5 seconds            | 0.5 seconds              |

- **Analysis of Findings:**
As the code originally is the improvement_factor wasn’t a large enough number to
address the gap between the actual response times and the expected times.
Additionally, code inefficiencies were found mainly pertaining to the
check_response_time functions which were called multiple times unnecessarily in
different functions. The round functions used to display results to the developer also
weren’t accurate enough in the case of move_arm for example.

---

## Optimizations and Solutions

- **Code modifications:**  
  Global variables, temporary variables, and conditionals were added to decrease the  
  time complexity to O(n). This is mainly shown in the optimized_command functions  
  which reduce time complexity from O(n^2) to O(n). Additionally, the improvement  
  factor was adjusted to fit the gap between the actual times and expected times.

- **Impact of Optimizations:** Recommend additional tests.  
  Change the round functions second parameter while doing future manual testing to get  
  accurate measurements. Provide an edge case test for each command to increase  
  reliability. Provide a program or UI that gives a direct comparison of response times for  
  faster diagnosis.

---

## Conclusion

- **Summary of Findings:**  
  The main factor providing the optimizations was inaccurate and there were code  
  inefficiencies in the control code. The optimizations calculated were adjusted to address  
  the actual response times along with more organized and manageable code being  
  introduced that prioritizes low cohesion and a better complexity.

- **Overall Impact:** Highlight how the changes improve performance and reliability for surgical procedures.  
  Faster time complexities and low coupling reduce the chances of having defects and  
  failures in control code due to code inefficiencies. Increasing the improvement factor  
  amount allows for optimizations that allow for faster response times over being  
  potentially behind which assures successful sessions with the device.

- **Next Steps:** Recommend further improvements.  
  The system should switch to being closed-loop for faster identification while additional  
  features should be introduced for preventing or managing the risks of processing  
  bottlenecks.
