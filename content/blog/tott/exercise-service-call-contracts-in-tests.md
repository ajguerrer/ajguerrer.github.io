+++
title = "Exercise Service Call Contracts in Tests"
date = 2018-11-27
weight = 2
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

If the code under test relies on the contract of a service, prefer exercising the service call
instead of mocking it out. Some service owners provide a fake. Otherwise, use a hermetic server.