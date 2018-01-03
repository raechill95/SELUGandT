---
title: LONI Login Activity
teaching: 0
exercises: 0
questions:
  - "What is a high-performance cluster computer?"
  - "Why is this useful for our work?"
objectives:
  - "Set up an account on the LONI computer."
  - "Log In to LONI."
  - "Understand how to use software on this machine."
---

# Making a LONI Account

LONI (Louisiana Optic Network Infrastructure) is a supercomputing network available to public institutions in Louisiana. 

Supercomputers, or high-performance cluster computers, are really just networks of computers that allow users to access enough processor power to do long jobs. Several of the computations we do this semester will be too complex or will take too long to run on your personal laptop or classroom machine, so we will send these computations to LONI.

LONI's main advantages:

* Free for use if you are an LA public institution student.
* Available on all platforms.
* Flexible to multiple software installations.
* Supports multiple programming paradigms.
* Very large community with good documentation.

## Making a LONI Account

I have already set up allocations for this course. But each of you needs to have an independent way to access those allocations. To do this, you will create a user account [here](https://allocations.loni.org/login_request.php). Please use your SELU email address for this step. Work through the steps. You should receive a confirmation email shortly about your account being set up. 

## Joining the Class Allocations

Once you have received an email confirming your account creation, go [here](https://allocations.loni.org/allocations.php?only_mach=Dell_Cluster). You will click on "Join an existing allocation," and enter my email (april.wright@selu.edu). Join the correct one for the class.

## Logging in to LONI.

Now, we will use our command line interfaces to log into the LONI cluster.

Follow the instructions under the Setup Tab to access your terminal. If you are on a lab machine ... 

Once the terminal is open, type 

```
ssh username@eric.loni.org
```

Replace 'username' with your username. You will be prompted to enter your LONI password. You may be asked if you would like to connect. Type 'Yes.'

If you successfully log in, you should see something like this:

![LONI Login Screen](../fig/LONI.png)
