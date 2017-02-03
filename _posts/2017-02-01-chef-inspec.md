---
layout: post
title:  "Is this the future of compliance?"
date:   2017-02-01 12:50:36 -0400
categories: devops, chef, compliance
---

## What is the problem?

At the company where I work now, my team has been asked by compliance to fill out a large number of paperworks. Document after documents, with a large amount of them being abstruse and irrelevant. I often wondered if really there is anyone reading them on the other side, because there were such a person, it would be a very tedious job.

![Compliance](https://encrypted-tbn2.gstatic.com/images?q=tbn:ANd9GcSsusFy6WnZQYy_wtxzFafRC_Hug5KzFdILNsoM0DQ1BhFQZnPAOg "Compliance Documentation")

Based on my experience, they can be ineffective. What is on paper may not be a valid representation of the system; even if it did, the actual system is more fluid.

## What is Inspec?

What [Inspec] does is to provide an interface for verifying a server's various aspects/attributes.

```ruby
describe port(80) do
  it { should be_listening }
end

describe windows_feature('Web-Server') do
  it { should be_installed }
end

describe iis_site('Default Web Site') do
  it { should exist }
  it { should be_running }
  it { should have_app_pool('DefaultAppPool') }
  it { should have_path('%SystemDrive%\inetpub\wwwroot') }
end
```

### How to run it?

the following commands are normally used:

* `kitchen converge`: applies the recipe
* `kitchen verify`:   verify the assertions
* `kitchen destroy`:  destroys the test server (local virtualbox)
* `kitchen test`:     all the above.

### Is it useful?

Where I feel Inspec shines is:

1. "compliance as code", so that we can avoid all the paper chase and automate the entire process.
2. It is independent of chef, and doesn't require any agent, such as chef client.
3. it can almost provide the utility of TDD (test driven development), especially if a `kitchen converge` takes place first, and any following change will trigger `kitchen converge && kitchen verify`. But yeah, it is still a little slow.

## Where can it be better?

I wish there would be a dashboard, where the policies and failures are displayed. Also,


[Inspec]: http://inspec.io/
