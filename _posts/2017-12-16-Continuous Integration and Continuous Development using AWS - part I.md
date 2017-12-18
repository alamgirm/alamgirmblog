---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Continuous Integration and Continuous Development on AWS : part I' 
date:   2017-12-16 20:18:00
tags: [CICD, AWS]
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---

<img src="/assets/images/2017/17_12_16/cicd-0.png"  alt="CICD" class="leftimg" />
DevOps philosophy has it that an application is better developed, tested and deployed in small pieces, in a continuous manner. This hopefully serves the changing requirements (of the clients or users) in both time and cost effective way. The developers, and operational team also always have something that is proved to be working, something to roll back to in case a change does not end successfully. To practically embrace the philosphy there needs to be an organizational pipeline where the teams (development, QA testing, deployment, monitoring etc) participate. And for better communication among the teams, most if not all teams use same kind of automation tools/platforms. This post is the first in a series of posts that talks about automation tools, employed on Amazon AWS cloud platform.

<!--more-->

### AWS Setup
The first thing someone should do is to create an IAM user, and not use the root or main AWS account. It is straightforward- login to your AWS root account at: console.aws.amazon.com
<img src="/assets/images/2017/17_12_16/aws-root-login.png"  alt="AWS Root login" class="leftimg" />
If you have multi-factor authentication (MFA) enabled, a second screen will be appearing asking for authentication code. After successful login appears the AWS dashboard that lists many of the services AWS offers. A searchbox is readily available to quickly navigate to a service. We need the IAM service. Clicking on IAM will take to the IAM dashboard:
<img src="/assets/images/2017/17_12_16/iam-dashboard.png"  alt="IAM dashboard" class="leftimg" />

We need to create a new user (with limited priviledges) for all the exercises related to DevOps tools. Click on `Users` on the left pane, then `Add User` on the top to initiate the user creation process.
<img src="/assets/images/2017/17_12_16/iam-add-user.png"  alt="IAM Add User" class="leftimg" />

I am using `DevOpUser` as user name, and checking both *Programmatic* and *AWS Management Console* access check boxes. I am letting AWS autogenerate a password, but not forcing the user to reset the password.  Hitting *Next* will take us to permissions.

On Permissions screen, I am choosing the *Attach existing policies directly* option. Typing EC2 in the search box, I choose `EC2FullAceess` and `S3FullAccess`. Then follw next to finally create the user. I click on **Download.csv** button to save the login credentials for  newly created user `DevOpUser`. Also take note of the login url:
`https://xxxxxx.signin.aws.amazon.com/console` (xxxxxx denotes string I removed for privacy).
<img src="/assets/images/2017/17_12_16/iam-add-user-done.png"  alt="IAM Add User" class="leftimg" />

To keep things organized on my own local machine (a MacBook), I created a folder named `DevOps` under my home directory. The whole patch should be `/Users/<username>/DevOps`. I copy the credential file downloaded in the last step into this directory.

### More AWS Setup
The user created above has full priviledge in using EC2 service. However, we need one more priviledge: access to `CloudFormation` so that we can create a set of EC2 instances from code, in an automated way. We go back to IAM service dashboard and click on `Users` on the left panel, then select the user `DevOpUser`. It should show a summary of the user, and a button at the bottom to add `inline policy`. I clicked on `Add inline policy` then chose `Custom Policy`, then finally pasted this piece of code in the editor:
<pre>
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:Describe*",
                "cloudformation:List*",
                "cloudformation:Get*",
                "cloudformation:Create*",
                "cloudformation:Delete*",
                "cloudformation:Update*",
                "cloudformation:Validate*"
            ],
            "Resource": "*"
        }
    ]
}
</pre>

I named the policy as `CloudFormationFullAccess`, then validated the policy before applying. Though validation should be enough, however if you want to be abolutely sure, there is an easy way to simulate the policies attached to the user. This simulation allows us to find out if the user has enough priviledges to do things it needs to. The simulator found at <a href="https://policysim.aws.amazon.com"> Amazon site </a>.

### Testing new User
The login url for (non-root) limited-rights user is different.  You shoul be able to see it immediately after creating the user. Generally it has the following format:
<pre>
https://Root_Account_ID.signin.aws.amazon.com/console/
</pre>
Where `Root_Accound_ID` is the either the account number or alias of the root account. Visiting the login link, and after providing the user name and password for `DevOpUser` I found this dashboard.
<img src="/assets/images/2017/17_12_16/iam-user-dashboard.png"  alt="User Dashboard" class="leftimg" />
This dashboard allows creating and managing AWS services. In addition to this dashboard, AWS allows creating and managing service resouces via programmatic access. In the next post we'll see how to create EC2 instances
using AWS CLI (command line  interface).


