# ZCI

## Interface

One click to run singlton service in localhost(docker run)

Can scale to large cluster

Setup post-commit hook to run test

Or setup by web ui like drone.io

Write a dockerfile to run test and easy to tell success or fail(how?)

Integrated with github for public service

Support send email for notification(Use smtp.SendMail)

Option to provide web hook for other service like deploy

Option to support i18n

Option to support mysql or sqlite or postgres


## Technoloy

Docker to run unit test

Kubernetes for docker cluster orchestration?

Use sqlite by default without no external requirement


## Learn

Learn drone about what library they use for yaml?

How they detect whether it fails or success?

Implement client tool with github.com/codegangsta/cli
