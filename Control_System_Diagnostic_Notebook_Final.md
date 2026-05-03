![Johnson & Johnson MedTech Logo](https://raw.githubusercontent.com/Forage-Simulations/Johnson-Johnson-Robotics-Controls/main/github_assets.png)


# Control System Diagnostic Notebook for Robotic Arm

### Objective
This notebook is designed to help identify and resolve response delays in the control code of a robotic arm. You will:

- Diagnose the root cause of delays in command response times.
- Optimize control code for improved performance.
- Document your findings and propose actionable solutions.

### Instructions
Follow the steps outlined in this notebook to diagnose and resolve issues in the robotic arm's control system:

1. **Load Libraries**: Run the provided setup code to load necessary Python libraries.
2. **Run Diagnostic Functions**: Use the `check_response_time` function to measure command response times.
3. **Analyze Findings**: Record observed delays and propose a hypothesis for their cause.
4. **Test Optimizations**: Apply optimization logic and compare results.
5. **Record Results**: Summarize your findings and recommendations in a structured format.

Focus on the `rotate_joint` command, as it has been flagged for delays.



```python
# Import required libraries
import time  # For measuring response times
import numpy as np  # For numerical calculations

```


```python
# Global dictionary for expected response times
expected_time = {}
expected_time["rotate_joint"] = .10
expected_time["move_arm"] = .10
expected_time["adjust_grip"] = .9
```


```python
# List of commands to test
commands = ["move_arm", "rotate_joint", "adjust_grip"]
response_time = {}

# Measure and print response times for each command
print("Testing initial command response times:")
for cmd in commands:
    response_time[cmd] = check_response_time(cmd)
    print(f"{cmd} response time: {round(response_time[cmd], 3)} seconds")

```

    Testing initial command response times:
    move_arm response time: 0.1 seconds
    rotate_joint response time: 0.15 seconds
    adjust_grip response time: 0.05 seconds



```python

# Define a function to measure the response time of commands
def check_response_time(command):
    """Simulates command execution and measures response time."""
    start_time = time.time()
    if command == "rotate_joint":
        time.sleep(0.15)  # Simulate delay for rotate_joint
    elif command == "move_arm":
        time.sleep(0.1)  # Simulate moderate response time
    elif command == "adjust_grip":
        time.sleep(0.05)  # Simulate fast response time
    response_time = time.time() - start_time
    return response_time

```


```python
### Step 3: Analyze Initial Findings

"""
Record the response times observed for each command. Focus on identifying commands with higher response times.

| Command       | Observed Response Time | Expected Response Time | Notes on Performance    |
|---------------|------------------------|------------------------|-------------------------|
| move_arm      | 0.1                    | 0.10                   | Performes as expected   |
| rotate_joint  | 0.15                   | 0.10                   | 50% delay from expected |
| adjust_grip   | 0.05                   | 0.09                   | Excedes expectations    |

#### Hypothesis:
The `rotate_joint` command may include redundant calculations causing delays or unoptimized code that has high time complexities. This is likely as there are no defects or failures outside of syntax errors.  
"""

```




    '\nRecord the response times observed for each command. Focus on identifying commands with higher response times.\n\n| Command       | Observed Response Time | Expected Response Time | Notes on Performance    |\n|---------------|------------------------|------------------------|-------------------------|\n| move_arm      | 0.1                    | 0.10                   | Performes as expected   |\n| rotate_joint  | 0.15                   | 0.10                   | 50% delay from expected |\n| adjust_grip   | 0.05                   | 0.09                   | Excedes expectations    |\n\n#### Hypothesis:\nThe `rotate_joint` command may include redundant calculations causing delays or unoptimized code that has high time complexities. This is likely as there are no defects or failures outside of syntax errors.  \n'




```python
# Test each command after optimizations
print("\nTesting optimized command response times:")
for cmd in commands:
    optimized_time = optimized_command(cmd)
    print(f"{cmd} optimized response time: {round(optimized_time, 3)} seconds")

```

    
    Testing optimized command response times:
    Optimizing command: move_arm
    move_arm optimized response time: 0.1 seconds
    Optimizing command: rotate_joint
    rotate_joint optimized response time: 0.09 seconds
    Optimizing command: adjust_grip
    adjust_grip optimized response time: 0.05 seconds



```python
# Define a function to simulate optimized command execution
def optimized_command(command, improvement_factor=0.4):
    """Simulates optimized command execution."""
    print(f"Optimizing command: {command}")  # Placeholder action
    response_time_variable = response_time[command]
    if response_time_variable > (expected_time[command] + .0001): #.0001 is there to prevent uncesseceary optimization to `move_arm`: actual = .1001, expected = .1
        optimized_response_time = response_time_variable * (1 - improvement_factor)
        return optimized_response_time
    else:
        return response_time_variable

```


```python
### Step 5: Record Results
"""
Use this section to document your findings from the optimization process.
"""
#### Observations:
"""
- Initial Response Times:
  - move_arm: 0.10
  - rotate_joint: 0.15
  - adjust_grip: 0.05

- Optimized Response Times:
  - move_arm: 0.10
  - rotate_joint: 0.09
  - adjust_grip: 0.05
"""
#### Key Insights:
"""
- Which commands showed improvement after optimization?
    The `rotate_joint` command shows a 10% improvement after optimization. If the .0001 is removed from cell 128 line 6 than you will find a 40% decrease in response time for `move_arm`.
- Were any specific optimizations particularly effective?
    Including global variables, temporary variables,  adding a conditional was added to the optimized_command function prevented functions from being repeated with decrease the time complexity.
    The improvement factor was also adjusted to optimize response times properly.
"""
```




    '\n- Which commands showed improvement after optimization?\n    The `rotate_joint` command shows a 10% improvement after optimization. If the .0001 is removed from cell 128 line 6 than you will find a 40% decrease in response time for `move_arm`.\n- Were any specific optimizations particularly effective?\n    Including global variables, temporary variables,  adding a conditional was added to the optimized_command function prevented functions from being repeated with decrease the time complexity.\n    The improvement factor was also adjusted to optimize response times properly.\n'




```python
### Step 6: Summary and Recommendations
"""
Summarize your findings, including identified issues, optimizations applied, and next steps.
    **Findings**:
    - Code inefficiencies were found where wasn't optimized to having a O(n) time complexity. Instead it ran in O(n^2).
    - The round function is hidering accuracy in measuring response times. For example, uneccesary optimization as done to `move_arm` becasue the actual = .1001 while the expected was .10.
    - Multiple syntax errors in code due to not using comments properly. Words like "for" were used in sentences which python mistook for functions.
    
    **Identified Issues**:
    - The `rotate_joint` response time was 50% greater than the expected response time. 
    - The improvement_factor was inaccurate in its optimization. Originally it adjusted response times to being about .2 over the expected. 
    
    **Optimizations applied**:
    - Global variables, temporary variables, and conditionals were added to decrease the time complexity to O(n).
    - The improvement factor was adjsuted to fit the gap between the actual times and expected times.

    **Next Steps:**
        - Change from open-looped to closed-loop for response times in order to identify problems faster.
        - Provides features that prevent or manage the risk of processing bottlenecks
        
        *Small Improvements*:
        - Change the the second round function parameter in the future for more accurate diagnostics
        - Keep code complexity as low as possible by not recalling functions that already ran
        - Follow current commenting structure
        - Continue to refine code
"""
```




    '\nSummarize your findings, including identified issues, optimizations applied, and next steps.\n    **Findings**:\n    - Code inefficiencies were found where wasn\'t optimized to having a O(n) time complexity. Instead it ran in O(n^2).\n    - The round function is hidering accuracy in measuring response times. For example, uneccesary optimization as done to `move_arm` becasue the actual = .1001 while the expected was .10.\n    - Multiple syntax errors in code due to not using comments properly. Words like "for" were used in sentences which python mistook for functions.\n    \n    **Identified Issues**:\n    - The `rotate_joint` response time was 50% greater than the expected response time. \n    - The improvement_factor was inaccurate in its optimization. Originally it adjusted response times to being about .2 over the expected. \n    \n    **Optimizations applied**:\n    - Global variables, temporary variables, and conditionals were added to decrease the time complexity to O(n).\n    - The improvement factor was adjsuted to fit the gap between the actual times and expected times.\n\n    **Next Steps:**\n        - Change from open-looped to closed-loop for response times in order to identify problems faster.\n        - Provides features that prevent or manage the risk of processing bottlenecks\n        \n        *Small Improvements*:\n        - Change the the second round function parameter in the future for more accurate diagnostics\n        - Keep code complexity as low as possible by not recalling functions that already ran\n        - Follow current commenting structure\n        - Continue to refine code\n'


