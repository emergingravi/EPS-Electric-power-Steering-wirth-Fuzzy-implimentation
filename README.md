# EPS-System-with-Fuzzy

This is project is designed to control the torque for electric power steering (EPS) systems in vehicles. EPS systems assist the driver in steering by reducing the effort needed to turn the steering wheel.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Algorithm Details](#algorithm-details)
  - [input and output varible](#variables)
  - [Membershp function](#membership_function)
  - [fuzzy rules](#Fuzzy_rules)
  - [Control system](Control_system)
  - [input values](input_values)
- [Results](#results)

## Introduction

This project demonstrates how to use the ESP (Electric Power Sterring) to reduce the effort needed to turn the sterring wheel according to the road condition . 

## Features

-Adaptive Steering Assistance
-Improved Driver Feel and Comfort
-Compensation for Road Conditions
-Enhancing Safety

## Installation (Google Colaboratary)

To run this project from google collab, follow these steps:

### 1. Clone the repository:
   open any folder in vscode
   ```bash
   git clone https://github.com/emergingravi/EPS-Electric-power-Steering-with-Fuzzy-implimentation
   ```

### 2. installation

    open your google drive
    right click
    select more - connect more apps
    search for google Colaboratory
    install
    

### 3. creating a new file:
    right click 
    select more - google colaboratary
        
### 4. connecting to server:

    select dropdown menu of connect
    change runtime
    select your desired hardware accelerator (TPU v2 -recommended)
    save
    click on connect

### 6. Run the algorithm:

   Follow the instructions within the colaboraotry to run the ESP_algorithm and view results.

### 7. Shutdown Colaboratory:

   Once finished, you can shutdown teh file by pressing `Ctrl + w` 


## Algorithm Details

### define input and output variables
```bash
speed = ctrl.Antecedent(np.arange(0, 101, 1), 'speed')  
steering_angle = ctrl.Antecedent(np.arange(-45, 46, 1), 'steering_angle')
torque_assistance = ctrl.Consequent(np.arange(0, 101, 1), 'torque_assistance')
```
### Membership Function
 ## membership function for speed
 ```bash
speed['low'] = fuzz.trimf(speed.universe, [0, 0, 50])
speed['medium'] = fuzz.trimf(speed.universe, [0, 50, 100])
speed['high'] = fuzz.trimf(speed.universe, [50, 100, 100])
```
 ## membership functions for steering angle
 ```bash
steering_angle['negative_large'] = fuzz.trimf(steering_angle.universe, [-45, -45, -15])
steering_angle['negative_small'] = fuzz.trimf(steering_angle.universe, [-30, -15, 0])
steering_angle['zero'] = fuzz.trimf(steering_angle.universe, [-5, 0, 5])
steering_angle['positive_small'] = fuzz.trimf(steering_angle.universe, [0, 15, 30])
steering_angle['positive_large'] = fuzz.trimf(steering_angle.universe, [15, 45, 45])
```
 ## membership functions for torque assistance
```bash
torque_assistance['low'] = fuzz.trimf(torque_assistance.universe, [0, 0, 50])
torque_assistance['medium'] = fuzz.trimf(torque_assistance.universe, [0, 50, 100])
torque_assistance['high'] = fuzz.trimf(torque_assistance.universe, [50, 100, 100])
```
### fuzzy rules
```bash
rules = [
    ctrl.Rule(speed['low'] & steering_angle['negative_large'], torque_assistance['high']),
    ctrl.Rule(speed['low'] & steering_angle['negative_small'], torque_assistance['high']),
    ctrl.Rule(speed['low'] & steering_angle['zero'], torque_assistance['medium']),
    ctrl.Rule(speed['low'] & steering_angle['positive_small'], torque_assistance['high']),
    ctrl.Rule(speed['low'] & steering_angle['positive_large'], torque_assistance['high']),

    ctrl.Rule(speed['medium'] & steering_angle['negative_large'], torque_assistance['medium']),
    ctrl.Rule(speed['medium'] & steering_angle['negative_small'], torque_assistance['medium']),
    ctrl.Rule(speed['medium'] & steering_angle['zero'], torque_assistance['low']),
    ctrl.Rule(speed['medium'] & steering_angle['positive_small'], torque_assistance['medium']),
    ctrl.Rule(speed['medium'] & steering_angle['positive_large'], torque_assistance['medium']),

    ctrl.Rule(speed['high'] & steering_angle['negative_large'], torque_assistance['low']),
    ctrl.Rule(speed['high'] & steering_angle['negative_small'], torque_assistance['low']),
    ctrl.Rule(speed['high'] & steering_angle['zero'], torque_assistance['low']),
    ctrl.Rule(speed['high'] & steering_angle['positive_small'], torque_assistance['low']),
    ctrl.Rule(speed['high'] & steering_angle['positive_large'], torque_assistance['low']),
]

```
### create the control system
```bash
torque_control = ctrl.ControlSystem(rules)
torque_simulation = ctrl.ControlSystemSimulation(torque_control)
```
### input values
```bash
input_speed =float(input("input the speed (0 to 100)  : ") ) # Example speed in km/h
input_steering_angle =float(input("enter the sterring angle (-45 to 45 ) : "))
torque_simulation.input['speed'] = input_speed
torque_simulation.input['steering_angle'] = input_steering_angle
```

### Results
  After running the algorithm the output for the torque assistance will be displayed 
  ```bash
torque_simulation.compute()
print(f"Torque assistance: {torque_simulation.output['torque_assistance']}%")
torque_assistance.view(sim=torque_simulation
```
  
```text
input the speed (0 to 100)  : 56
enter the sterring angle (-45 to 45 ) : 20
Torque assistance: 49.61432397579605%
```

