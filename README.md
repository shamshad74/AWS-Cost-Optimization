# AWS-Cost-Optimization

# Efficient AWS Cost Management through Stale Resource Detection

## problem :
Sometimes, developers create EC2 instances with volumes attached to them by default. For backup purposes, these developers also create snapshots. However, when they no longer need the EC2 instance and decide to terminate it, they sometimes forget to delete the snapshots created for backup. As a result, they continue to incur costs for these unused snapshots, even though they are not actively using them.

## Solution :
We're using AWS to save money on storage costs. We made a Smart Lambda function that looks at our snapshots and our EC2 instances. If Lambda finds a snapshot that isn't connected to any active EC2 instances, it deletes it to save us money. This helps us keep our AWS costs down.

## Note :
There are many similar problems like this. For instance, we might attach an Elastic IP to our EC2 instance but forget to delete the Elastic IP after terminating the EC2 instance. In such a case, the Elastic IP continues to incur costs for us.

## Steps :
### step 1 :
1. Log into your AWS Console.
2. Navigate to the EC2 Console.
3. In the Instances section, select 'Instances,' and then click on 'Launch Instance'.

![Screenshot 2024-01-29 183923](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/76d7eddb-c95e-4666-a187-bf68debc791c)

4. In the Instances section, select 'Instances,' and then click on 'Launch Instance'.

![Screenshot 2024-01-29 184453](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/73d424f0-0f3e-46a4-a387-82e04477d915)

5. You will notice that a default volume has already been created for us.
6. Next, click on 'Snapshots,' and then click the 'Create Snapshot' button. It will prompt you with a page that looks like this.

![Screenshot 2024-01-29 184529](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/7e2efdf9-796a-402a-b891-d8562d017a8f)

7. In Volume ID section choose your default Volume ID created when we create instance.
8. Next, click 'Next,' provide a name for your Snapshot, and then scroll down and click 'Create Snapshot'.

![Screenshot 2024-01-29 184553](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/bc2519da-b938-4822-9ad7-3599934fda06)

### Step 2 :
1. After creating a Snapshot, navigate to the Lambda Console..
2. You will see some options in the user interface, such as 'Create Function'.
3. Click on 'Functions'.

![Screenshot 2024-01-29 184618](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/7749faa8-0fe3-44d0-b919-dc157063d2ba)

4. Select 'Author from Scratch,' then enter the Function name, and choose the latest Python version.
5. Scroll down and click 'Create Function'.
6. After creating the function, scroll down, and you will see something like the image below.

![Screenshot 2024-01-29 184700](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/c0a77d14-0ff8-4419-bbd7-fcb766dc7ad5)

7. Click on the 'Code' section.
8. Next, clear the existing code and replace it with the 'identify_stale_snapshots.py' code.

![Screenshot 2024-01-29 184737](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/567487e1-4349-44ba-bf5c-89609ba07638)

9. Click 'Deploy' to save your changes, and then click 'Test.' It will prompt a page that looks like the one given below.

![Screenshot 2024-01-29 184818](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/c4d5e384-7cbf-46e6-9989-d639871ec3d6)

10. Please configure the settings as displayed above and then scroll down. Next, click on 'Create Event'.
11. Once you've created the event, proceed to the IAM Console(Identity and Access Management) and then navigate policies section to create a new policy.

![Screenshot 2024-01-30 124153](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/801a2a94-b453-4bc7-ade5-cf805d343b25)

12. Select the service as 'EC2'

![Screenshot 2024-01-30 124305](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/7e4cfcd2-2810-41a4-bf15-48f944eeeb52)

13. In the 'Actions' section, grant permissions for the following actions: DescribeInstances, DescribeVolumes, DescribeSnapshots, DeleteSnapshots.
14. After Creating the Policies , Navigate to the Lambda Sections.
15. Next, go to the page of the Lambda function you've created. In the "Permissions" section, click on the role name.

![Screenshot 2024-01-30 124332](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/b0374612-eda4-4546-92f9-5c58d163ba98)

16. Click on 'Add Permissions' and then select 'Attach Policy.'

![Screenshot 2024-01-30 124407](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/4decf4d1-59e7-4fc2-a2fa-a4f67978c3ad)

17. Choose the correct policy you created.

![Screenshot 2024-01-30 124438](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/c2881e22-23de-4864-bb94-335c77d0524d)

18. Then scroll down and click 'Add Permissions'.
19. After that, you can go to the Lambda function page and run the code; it will display some outputs as shown below.

![Screenshot 2024-01-30 124516](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/9b65174a-31c8-4857-991f-210666e86193)

### Step 3 :
1. You can terminate the EC2 instance to test our Lambda function.
2. Navigate to the EC2 console and then terminate the EC2 instance.
3. Return to the Lambda console to test the code; go to the Lambda Function page.
4. Under the Code section, click 'Test code', it will display an output like this.

![Screenshot 2024-01-30 124550](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/b19413b5-501c-4b14-a78a-132e2b3596d2)

5. As expected, our Lambda function deleted the snapshot because it was associated with a volume that couldn't be found.

# Additional notes:
We can use CloudWatch to automatically trigger the Lambda function every hour, day, minute, or second. However, this may result in higher costs because our Lambda execution time increases when triggered automatically. Nevertheless, manually triggering this function is a better choice because it allows us to trigger it when needed.

# CloudWatch or EventBridge Implementation :
### Steps :
1. Navigate to CloudWatch Console.

![Screenshot 2024-01-30 133944](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/0181b8b6-6b66-482a-805c-b615dc7ea1fd)

![Screenshot 2024-01-30 134100](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/208b027e-b25a-4289-889e-e65d84b2ee27)

![Screenshot 2024-01-30 134139](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/f15e6fcd-2c72-4f2f-8401-bd907deb2d01)

![Screenshot 2024-01-30 134208](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/22caca86-dc68-45e5-8ba6-c4f79c5fd2c4)

2. Next, on the following page, configure the schedule pattern as follows:

![Screenshot 2024-01-30 134312](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/3643467c-df96-424b-9489-24cdafbe3ce7)

3. Scroll Down and then Click Next.

![Screenshot 2024-01-30 134451](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/410461e5-ec02-4e64-98ee-4f86e20f8b82)

![Screenshot 2024-01-30 134539](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/a3db6307-5c08-404a-997d-3d7cc86e8626)

4. Scroll Down and then Click Next.
5. On the next page, choose 'None' for the 'Action after Schedule' option.

![Screenshot 2024-01-30 134631](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/a56cf72f-8632-4a8b-a360-c6200082594e)

![Screenshot 2024-01-30 134655](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/5714dc14-0447-4b1c-aa4b-9b36c89adb6d)

![Screenshot 2024-01-30 134731](https://github.com/shamshad74/AWS-Cost-Optimization/assets/117065471/c4ef9669-be5c-4cec-91f6-4bb8acff9149)

6. You have successfully created the scheduler, which will trigger the Lambda function every hour.
7. However, please note that this setup will incur some costs since the function is triggered continuously every hour. Alternatively, we can configure it to run on specific days and times as needed.





































