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



16. Click on 'Add Permissions' and then select 'Attach Policy.'



17. Choose the correct policy you created.
18. Then scroll down and click 'Add Permissions'.
19. After that, you can go to the Lambda function page and run the code; it will display some outputs as shown below.
    















