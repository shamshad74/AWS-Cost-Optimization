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















