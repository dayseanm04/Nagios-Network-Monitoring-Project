# 09 – Create Custom Nagios Templates [Time Sensitive Service Template]

In this doc, I will create a separate folder and file for my own Nagios templates, so they're kept apart from the default templates that come with Nagios. Then I'll create my first custom template, **`time-sensitive`**, for high-priority service checks like CPU, memory, and disk that need to be checked more often than the default.

---

## What Is a Template?

A template is a blueprint of settings that hosts and services can inherit with the **`use`** line, so I don't have to repeat the same settings in every definition. 

## Why a Separate File?

