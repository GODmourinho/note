# Cinder

Volumn is the block device

check_volumn and reserve_volumn has no lock

Don't handle update db error, no retry logic

The problem "Attaching" not chnage to "Attached", not found

## Architecture

Scheduler pick the best volume for it

api -> scheduler -> volumn

## Queue

Exchange the message to different queues

Routing key match the message. Topic is the regular expression.

send message to the exchange




