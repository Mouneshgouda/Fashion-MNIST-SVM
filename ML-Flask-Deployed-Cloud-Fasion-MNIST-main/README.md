# ML-Flask-Deployed-Cloud-Fasion-MNIST

Deploy Flask Machine Learning Application on AWS EC2.

We used the Fashion MNIST dataset provided by [Zalando Research](https://www.kaggle.com/zalando-research/fashionmnist)
## Introduction
Welcome to Fashion MNIST classification system built by Chenyue Liang and Qianqian Che. Our web interface is clean and visually pleasing to users where users can upload image of clothes to get classification result for the genre. There are ten genres: T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot This website is deployed onto AWS EC2 with flask app and has Tensorflow CNN Machine Learning model for the backend. Front end scripts include HTML and Javascript. We had an emphasis on AWS deployment and CI/CD to better configure and change this project along the way. We struggled with some problems during dumping the model and deploying the model, and we will list them along with their solutions at the end. You can find the live URL for our website and instructions for running and setting up continuous deployment on AWS below. Also don't hesitate to watch our demo and presentation videos below for a walkthrough.

Live Website URL: http://ec2-3-19-218-40.us-east-2.compute.amazonaws.com:8080

![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/weblook.png)

## Youtube Links
* Live demo and code walkthrough: https://youtu.be/u8NQHpXrR5E
* Overview and structure of project: https://youtu.be/RqU8cM1YdWM

## Instructions for Website Navigation
* Step 1: Click "Upload Image" to upload a .png or .jpg format image of clothe
* Step 2: Click classify to proceed
* Step 3: The website moves to second webpage to display the genre of the clothe in the image
* Step 4: Click "Try another picture" to go back to previous page if you would like to classify another clothe image
## Architecture
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/flowChart.png)

## To Run Locally, Follow These Steps
- Step 1: Clone github repository:
```python
git clone https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST.git
```
- Step 2: Change direction to cloned repo
```python
cd ML-Flask-Deployed-Cloud-Fasion-MNIST/
```
- Step 3: Install requirements
```python
make install
```
- Stpe 4: Run flask app locally and copy the link displayed on console to open in a browser for our website
```python
python3 app.py
```
## Create AWS EC2 instance
- Fllow instructions of this document to create and launch EC2 instance
Create EC2 Instance: https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html
- Connect to your EC2 instance
```python
sudo chmod 600 fashion.pem
```
```python
ssh -i "fashion.pem" ubuntu@ec2<your ec2>.compute.amazonaws.com
```
```python
make install
```
```python
screen -R deploy python3 app.py
```
## Set up Continuous Integration/Continuous Deployment
- Refereed to this Document to Deploy Flask App on EC2a: https://www.twilio.com/blog/deploy-flask-python-app-aws
- We chose Ubuntu server, add 8 Gib storage when setup. 
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/ec2Instance.png)
- When setting security group, "Type" was set to all type, and "Source" is anywhere to ensure everyone can have access.
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/securityGroup.png)
- After saving fashion.pem and ssh it to the EC2 instance,
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/keyPair.png)
use: 
```python
ssh ubuntu@<YOUR_IP_ADDRESS> 
```
to login to the Ubuntu server
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/ubuntuLogin.png)
- Inside the Ubuntu shell, first, install tmux and other specified requirements on the Ubuntu shell. 
```python
sudo apt update
sudo apt install python3 python3-pip tmux htop
```
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/tmuxInstall.png)
- Then, create a directory for the application that you want to deploy.
```python
mkdir deployFashionApp
```
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/projectFolder.png)
- Open another terminal, cd to the project folder
- In the project folder, make sure to have a requirements.txt file . If not, try:
```python
pip freeze > requirements.txt
```
- Copy the full path of the project folder
- Use 
```python
sudo rsync -rv <FULL_PATH>/ ubuntu@<YOUR_IP_ADDRESS>:/home/ubuntu/deployedapp
```
to transfer local project to ubuntu server. In our case, the complete command is:
```python
sudo rsync -rv /Users/cheryl/Documents/Data_Systems_Project/ML-Flask-Deployed-Cloud-Fasion-MNIST/ ubuntu@3.19.218.40:/home/ubuntu/deployFashionApp
```
- Go back to the Ubuntu Shell, cd to deployFashionApp
- Use tmux to keep the app runnning in background. First, create a new session, runningApp is the name of the session:
```python
tmux new -s runningApp
```
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/inSession.png)
- after been redirected to the tmux session, install all requirements for the app:
```python
pip3 install -r requirements.txt
```
- Run the app:
```python
python3 app.py
```
, then the link 8080 is created. replace the 0.0.0.0 with server ip address
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/runInSession.png)
- To keep this session running, detach it by Pressing control+B, release, than press D
![alt text](https://github.com/irische/ML-Flask-Deployed-Cloud-Fasion-MNIST/blob/main/screenshots/detach.png)
- Now the app is ready for Continuous Deployment! Loguo the server would not influence access to the website. If want to go back to the session later, attach to the session created earlier:
```python
tmux attach -t runningApp
```
