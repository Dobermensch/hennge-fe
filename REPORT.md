# Report

- First Commit: 
	- Created new project
- Second Commit:
	- Explored different choices for datepicker components. Experimented with LitePicker but decided to go with V-Calender. 
	- Created DatePicker Component.
	- Added Vue SVG Loader to load SVGs as components
	- Created Mail Component
- Third Commit: 
	- Added EventBus to emit **dateChanged** event from DatePicker Component to Mail Component.
	- Added functionality to Mail Component such as listening to event from DatePicker and filtering result according to it and sorting results based on clicked title.
-	Fourth Commit :
	-	Added Improvements.md && report.md
	-	Added function call once DatePicker Component mounts so it is filters for messages received on the particular date.