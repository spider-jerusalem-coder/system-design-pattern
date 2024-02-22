# Task runners

## Intent
* Offloading jobs which are not required in sync with request

## High level desigb
* Application service --> Broker --> Consumer 

## Security
* Malicious code effecting node resources
* Fix using dockerized container
* Fix using cgroups
* Fix using auth and authotization

## Usage pattern
* Flash sale
    * Sending emails when account is created
    * Processing images and videos
    * Rebuilding search indexes
    * Cleaning up stale data
