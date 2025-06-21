main -  secondary airguards

FrontEnd:
- Sidebar menu (Design - Manage - Unlock - Setup Device )

location of the AP is based on the GPS pulled from the device itself (POST from python script  from the gps module - GET in the frontend - POST to rescan)


Manage: Device Inventory (Device Name, Status(Active, Inactive, Health Check+ Locked, Unlocked), Health Check In case the item is unlocked) Items can be Airguard and all other networking related items

Reports should be inside the Dashboard page


Manage: Logs - 


Data:
- Users : 
	- Add Users
	- Subscribed (Add Subscription, Stop Subscription -> Survey)
	- Sub Users + Role Based Authentication
	- FrontEnd Case : Initiate Setup phase 
- Inventory Section:
	- Filtered: 
		- Network 
		- Airguard/managed devices 
		- unlocked/locked 
		Added Data : Barcode - serial number (when inputted the device becomes unlocked and added to the network (specified by the user)) Button that will initiate the setups phase
- Dashboard:
	- [[SNMP]] + Calculation

- Setup Phase (Airguard)
	- Device Nickname
	- Serial Number
	- IP 
	- Mac Address
	- Gyro
	- Location
	- Software algorithm version
- Setup Phase ():
	- username - password (For SSH reasons automated ssh key)
	- [[TODO Airguard]]
		


NOTE: the setup phase should be localhosted on the airguard itself, in setup phase it'll be openned on the the phone or the laptop a button must be pressed called transfer, this will take the data from the airguard to the cloud same website but on the cloud

Connection:
- add account internet on the airguard itself
- ethernet local internet connection
- sim card
- post to the phone then transfer