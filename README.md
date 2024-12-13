#include <iostream>
#include <fstream>
#include <string>
using namespace std;
//patients 
class Patient {
public:
	int age;
	int	ID;
	string name;
	string contact;
	string disease;
	Patient* next;

	Patient(string nme, int id, int a, string cn, string d) {
		name = nme;
		ID = id;
		age = a;
		contact = cn;
		disease = d;
		next = NULL;
	}
};
class patientList {
private:
	// private head pointer
	Patient* Head = NULL;
public:
	//file handling
	void saveToFile() {
		if (!Head) {
			cout << "No patient to save!" << endl;
			return;
		}
		ofstream patient("Patient.txt", ios::trunc);
		Patient* temp = Head;
		while (temp != NULL) {
			patient << temp->ID << "\n" << temp->age << "\n"
				<< temp->name << "\n" << temp->contact << "\n"
				<< temp->disease << "\n";
			temp = temp->next;
		}
		patient.close();
		cout << "All patients saved successfully." << endl;
	}
	void loadFile() {
		ifstream patient("Patient.txt");
		if (!patient.is_open()) {
			cout << "Error: Unable to open the file!" << endl;
			return;
		}

		string phone, disease, name;
		int id, age;

		while (patient >> id >> age) {
			patient.ignore();  // Ignore the newline after the class
			getline(patient, name);
			getline(patient, phone);
			getline(patient, disease);

			// Validate if all fields were successfully read
			if (patient.fail()) {
				cout << "Error: Corrupted file format or insufficient data!" << endl;
				break;
			}

			insertPatient(name, id, age, phone, disease);
		}

		patient.close();
		cout << "Data successfully loaded from file!" << endl;
	}
	string insertPatient(string nme, int id, int a, string cn, string d) {
		Patient* newNode = new Patient(nme, id, a, cn, d);
		if (Head == NULL) {
			Head = newNode;
		}
		else {
			Patient* temp = Head;
			while (temp->next != NULL) {
				temp = temp->next;
			}
			temp->next = newNode;
		}
		return d;
	}
	//deleting operation
	void deleteAtStart() {
		if (Head == NULL) {

			cout << "list is empty";
		}
		else {
			Patient* temp = Head;
			Head = Head->next;
			delete temp;
			saveToFile();
		}
	}
	void deleteAtEnd() {
		if (Head == NULL) {

			cout << "list is empty";
		}
		else {
			Patient* temp = Head;
			while (temp->next->next != NULL) {
				temp = temp->next;
			}
			delete temp->next;
			temp->next = NULL;
			saveToFile();
		}
	}
	void delete_by_id()
	{
		int a;
		cout << "enter the id\n";
		cin >> a;
		if (Head != NULL)
		{
			if (Head->ID == a)
			{
				deleteAtStart();
			}
			else
			{
				Patient* temp = Head;
				Patient* temp2 = Head;
				while (temp != NULL && temp->ID != a)
				{
					temp2 = temp;
					temp = temp->next;

				}
				if (temp != NULL)
				{
					temp2->next = temp->next;
					delete temp;
					temp = NULL;
					saveToFile();
				}
			}
		}
	}
	//search
	void searchPatient(string n) {
		Patient* temp = Head;

		// Check if the list is empty
		if (temp == NULL) {
			cout << "The patient list is empty.\n";
			return;
		}


		while (temp != NULL) {
			if (temp->name == n) {

				cout << "Name: " << temp->name << "   ";
				cout << "Patient ID: " << temp->ID << "   ";
				cout << "Age: " << temp->age << "   ";
				cout << "Contact No: " << temp->contact << endl;
				return;
			}
			temp = temp->next;
		}
		cout << "Patient with name \"" << n << "\" not found.\n";
	}
	void updateList() {
		int id;
		cout << "enter id:";
		cin >> id;
		Patient* temp = Head;
		while (temp != nullptr) {

			if (id == temp->ID) {
				cout << "found in record" << endl;
				cout << "Name: " << temp->name << "   ";
				cout << "Patient ID: " << temp->ID << "   ";
				cout << "Age: " << temp->age << "   " << endl;
				cout << "1. Update" << endl;
				cout << "2. Exit" << endl;
				int ch;
				cin >> ch;
				if (ch == 1) {
					cout << "enter patient name: " << endl;
					cin >> temp->name;
					cout << "enter age: " << endl;
					cin >> temp->age;
					cout << "enter contact no: " << endl;
					cin >> temp->contact;
					saveToFile();
				}
				if (ch == 2) {
					cout << "exit" << endl;
					cout << "the current  list is" << endl;
				}
			}

			temp = temp->next;
		}

	};
	//display 
	void display() {
		Patient* temp = Head;
		if (temp == NULL) {
			cout << "no patients to display" << endl;
			return;
		}
		while (temp != NULL) {
			cout << "Name :" << temp->name << "   ";
			cout << "patient ID :" << temp->ID << "   ";
			cout << "age :" << temp->age << "   ";
			cout << "contact no:" << temp->contact << endl;
			temp = temp->next;
		}

	}
};
//Doctors
class Doctor {
public:
	int id;
	int room;
	int P_count;
	string name;
	string specialization;
	string contact;
	string scedule;
	Doctor* next;
	Doctor(int i, int r, int p, string n, string sp, string c, string sc) {
		id = i;
		room = r;
		P_count = p;
		name = n;
		specialization = sp;
		contact = c;
		scedule = sc;
		next = NULL;

	}

};
class doctorList : public patientList {
	Doctor* Head = NULL;
public:
	//file handling
	void saveToFile() {
		if (!Head) {
			cout << "No doctor to save!" << endl;
			return;
		}
		ofstream doctor("doctor.txt", ios::trunc);
		Doctor* temp = Head;
		while (temp) {
			doctor << temp->id << "\n" << temp->room << "\n"
				<< temp->P_count << "\n" << temp->contact << "\n" << temp->name << "\n"
				<< temp->scedule << "\n" << temp->specialization << "\n";
			temp = temp->next;
		}
		doctor.close();
		cout << "All doctors saved successfully." << endl;
	}
	void loadFile() {
		ifstream doctor("Doctor.txt");
		if (!doctor.is_open()) {
			cout << "Error: Unable to open the file!" << endl;
			return;
		}

		string phone, scedule, name, specialization;
		int id, p_count, room;

		while (doctor >> id >> room >> p_count) {
			doctor.ignore();  // Ignore the newline after the class

			getline(doctor, phone);
			getline(doctor, name);
			getline(doctor, scedule);
			getline(doctor, specialization);

			// Validate if all fields were successfully read
			if (doctor.fail()) {
				cout << "Error: Corrupted file format or insufficient data!" << endl;
				break;
			}

			insertDoctor(id, room, p_count, name, phone, specialization, scedule);
		}

		doctor.close();
		cout << "Data successfully loaded from file!" << endl;
	}
	// Insserting Doc 
	void insertDoctor(int i, int r, int p, string n, string sp, string c, string sc) {
		Doctor* newNode = new Doctor(i, r, p, n, sp, c, sc);
		if (Head == NULL) {
			Head = newNode;
		}
		else {
			Doctor* temp = Head;
			while (temp->next != NULL) {
				temp = temp->next;
			}
			temp->next = newNode;
		}
	}
	//assigning
	void Assign() {
		string name, contact, disease;
		int id, age;

		// Get patient details from the user
		cout << "Enter patient's name: ";
		cin >> name;
		cout << "Enter patient ID: ";
		cin >> id;
		cout << "Enter patient's age: ";
		cin >> age;
		cout << "Enter patient's contact: ";
		cin >> contact;
		cout << "Enter patient's disease: ";
		cin >> disease;

		// Insert the patient into the list
		string key = patientList::insertPatient(name, id, age, contact, disease);

		// specilization based on disease
		string specialist;
		if (key == "Heart_attack" || key == "hypertension") {
			specialist = "Cardiologist";
		}
		else if (key == "asthma" || key == "allergies") {
			specialist = "allergist";
		}
		else if (key == "skin") {
			specialist = "dermatologist";
		}
		else if (key == "migraine") {
			specialist = "neurologist";
		}
		else if (key == "bones") {
			specialist = "orthopethic";
		}
		else if (key == "diabetes") {
			specialist = "Endocrinologist";
		}
		else {
			specialist = "general_physician";
		}

		// Find a doctor 
		Doctor* temp = Head;
		while (temp != NULL) {
			if (temp->specialization == specialist) {
				cout << "Assigned Doctor:\n";
				cout << "Name: " << temp->name << "\n";
				cout << "ID: " << temp->id << "\n";
				cout << "Specialization: " << temp->specialization << "\n";
				cout << "Room: " << temp->room << "\n";
				cout << "scedule :" << temp->scedule << endl;
				cout << "scedule :" << temp->contact << endl;
				return;
			}
			temp = temp->next;
		}

		cout << "No doctor available with specialization in " << specialist << endl;
	}

	// delete  Doc
	void deleteAtStart() {
		if (Head == NULL) {

			cout << "list is empty";
		}
		else {
			Doctor* temp = Head;
			Head = Head->next;
			delete temp;
			saveToFile();
		}
	}
	void deleteAtEnd() {
		if (Head == NULL) {

			cout << "list is empty";
		}
		else {
			Doctor* temp = Head;
			while (temp->next->next != NULL) {
				temp = temp->next;
			}
			delete temp->next;
			temp->next = NULL;
			saveToFile();
		}
	}
	void delete_by_id()
	{
		int ID;
		cout << "enter id to delete" << endl;
		cin >> ID;
		if (Head != NULL)
		{
			if (Head->id == ID)
			{
				deleteAtStart();
			}
			else
			{
				Doctor* temp = Head;
				Doctor* temp2 = Head;
				while (temp != NULL && temp->id != ID)
				{
					temp2 = temp;
					temp = temp->next;

				}
				if (temp != NULL)
				{
					temp2->next = temp->next;
					delete temp;
					temp = NULL;
					saveToFile();
				}
			}
		}
	}
	// Update  Information 
	void updateList() {
		int Id;
		cout << "enter id to search and update:";
		cin >> Id;
		Doctor* temp = Head;
		while (temp != nullptr) {

			if (Id == temp->id) {
				cout << "Name :" << temp->name << "   ";
				cout << "doctor ID :" << temp->id << "   ";
				cout << "room :" << temp->room << "   ";
				cout << "patient count :" << temp->P_count << "   ";
				cout << "specialization :" << temp->specialization << "   ";
				cout << "schedule :" << temp->scedule << "   ";
				cout << "contact no:" << temp->contact << endl;
				cout << "1. Update" << endl;
				cout << "2. Exit" << endl;
				int ch;
				cin >> ch;
				if (ch == 1) {
					cout << "enter doctor name: " << endl;
					cin >> temp->name;
					cout << "enter specialization: " << endl;
					cin >> temp->specialization;
					cout << "enter contact no: " << endl;
					cin >> temp->contact;
					cout << "enter patients  count: " << endl;
					cin >> temp->P_count;
					cout << "schedule :" << endl;
					cin >> temp->scedule;
					saveToFile();
				}
				if (ch == 2) {
					cout << "exit" << endl;
					cout << "the current  list is" << endl;
				}
			}

			temp = temp->next;
		}


	};
	// Availibility check
	void availableDoctors() {
		string spz;
		cout << "Enter specialization of doctor: " << endl;
		cin >> spz;
		Doctor* temp = Head;
		bool found = false;
		cout << "Available doctors with specialization in " << spz << ":\n";

		while (temp != NULL) {
			if (temp->specialization == spz) {
				cout << "Name::" << temp->name << "\t ID::" << temp->id << "\t Room:: " << temp->room << "\t Contact:: " << temp->contact << endl;
				found = true;
			}
			temp = temp->next;
		}
		if (!found) {
			cout << "No doctors found with specialization in " << spz << endl;
		}
	}
	// display 
	void display() {
		Doctor* temp = Head;
		if (temp == NULL) {
			cout << "no doctors to display" << endl;
			return;
		}
		while (temp != NULL) {
			cout << endl;
			cout << "Name :" << temp->name << "   ";
			cout << "doctor ID :" << temp->id << "   ";
			cout << "room :" << temp->room << "   ";
			cout << "patient count :" << temp->P_count << "   " << endl;
			cout << "specialization :" << temp->specialization << "   ";
			cout << "schedule :" << temp->scedule << "   ";
			cout << "contact no:" << temp->contact << endl;

			temp = temp->next;
		}

	}

};
//appointments
class Appointment {
public:
	int patientID;
	int doctorID;
	string time;
	Appointment* next;

	Appointment(int p, int d, string t) {
		patientID = p;
		doctorID = d;
		time = t;
		next = NULL;
	}
};
class appointmentQueue {
private:
	Appointment* front;
	Appointment* rear;
public:
	appointmentQueue() {
		front = NULL;
		rear = NULL;
	}
	void enqueue(int patientID, int doctorID, string time) {
		Appointment* newAppointment = new Appointment(patientID, doctorID, time);
		if (rear == NULL) {
			front = rear = newAppointment;
		}
		else {
			rear->next = newAppointment;
			rear = newAppointment;
		}
		cout << "Appointment scheduled." << endl;
	}
	void dequeue() {
		if (front != NULL) {
			Appointment* temp = front;
			front = front->next;
			delete temp;
		}
		cout << "Appointment completed" << endl;
	}
	void displayAppointments() {
		Appointment* temp = front;
		while (temp != NULL) {
			cout << "Patient ID: " << temp->patientID << endl;
			cout << "doctor ID: " << temp->doctorID << endl;
			cout << "time: " << temp->time << endl;
			temp = temp->next;
		}
	}
};
//medicationss
class Medication {
public:
	int   patientID;
	string medName;
	string dosage;
	Medication* next;

	Medication(int pID, string mName, string d) {
		patientID = pID;
		medName = mName;
		dosage = d;
		next = NULL;
	}
};
class MedicationStack {
private:
	Medication* top;

public:
	MedicationStack() {
		top = NULL;
	}
	void push(int patientID, string medName, string dosage) {
		Medication* newMed = new Medication(patientID, medName, dosage);
		newMed->next = top;
		top = newMed;
		cout << "Medication added." << endl;
	}
	void pop() {
		if (top != NULL) {
			Medication* temp = top;
			top = top->next;
			delete temp;
			cout << "Medication removed." << endl;
		}

	}
	void displayMedications() {
		Medication* temp = top;
		while (temp) {
			cout << "Patient ID: " << temp->patientID << endl;
			cout << "Medication: " << temp->medName << endl;
			cout << "Dosage: " << temp->dosage << endl;
			temp = temp->next;
		}
	}
};
class Billing {
public:
	double calculateBill(int daysAdmitted, int medsCost) {
		double total = daysAdmitted * 1000;  // Example cost per day

		total += medsCost;                   // Medication cost
		return total;
	}

	void displayBill(int patientID, int daysAdmitted, int medsCost) {
		double bill = calculateBill(daysAdmitted, medsCost);
		cout << "Patient ID: " << patientID << "\n";
		cout << "Days Admitted: " << daysAdmitted << ", Medications Cost: " << medsCost << "\n";
		cout << "Total Bill: $" << bill << "\n";
	}
};

void displayPatientMenu(patientList& patients) {
	int choice;
	do {
		cout << "\n--- Patient Menu ---\n";
		cout << "1. Register as a Patient\n";
		cout << "2. Search Patient by Name\n";
		cout << "3. Delete Patient by ID\n";
		cout << "4. Display Patient List\n";
		cout << "5. Go Back to main menu\n";
		cout << "Enter your choice: ";
		cin >> choice;

		switch (choice) {
		case 1: {
			string name, contact, disease;
			int id, age;
			cout << "Enter name: ";
			cin >> name;
			cout << "Enter ID: ";
			cin >> id;
			cout << "Enter age: ";
			cin >> age;
			cout << "Enter contact: ";
			cin >> contact;
			cout << "Enter disease: ";
			cin >> disease;
			patients.insertPatient(name, id, age, contact, disease);
			patients.saveToFile();
			cout << "Patient registered successfully!\n";
			break;
		}
		case 2: {
			string name;
			cout << "Enter the name to search: ";
			cin >> name;
			patients.searchPatient(name);
			break;
		}
		case 3:
			patients.delete_by_id();
			break;
		case 4:
			patients.display();
			break;
		case 5:
			cout << "Returning to main menu...\n";
			break;
		default:
			cout << "Invalid choice. Try again.\n";
		}
	} while (choice != 5);
}

void displayDoctorMenu(doctorList& doctors) {
	int choice;
	do {
		cout << "\n--- Doctor Menu ---\n";
		cout << "1. Add a Doctor\n";
		cout << "2. Display Doctor List\n";
		cout << "3. Search Available Doctors by Specialization\n";
		cout << "4. Assign a Patient to a Doctor\n";
		cout << "5. Update Doctor Information\n";
		cout << "6. Delete Doctor by ID\n";
		cout << "7. Go Back to main menu\n";
		cout << "Enter your choice: ";
		cin >> choice;

		switch (choice) {
		case 1: {
			int id, room, pCount;
			string name, specialization, contact, schedule;
			cout << "Enter doctor ID: ";
			cin >> id;
			cout << "Enter room number: ";
			cin >> room;
			cout << "Enter patient count: ";
			cin >> pCount;
			cout << "Enter doctor name: ";
			cin >> name;
			cout << "Enter specialization: ";
			cin >> specialization;
			cout << "Enter contact: ";
			cin >> contact;
			cout << "Enter schedule: ";
			cin >> schedule;
			doctors.insertDoctor(id, room, pCount, name, specialization, contact, schedule);
			doctors.saveToFile();
			cout << "Doctor added successfully!\n";
			break;
		}
		case 2:
			doctors.display();
			break;
		case 3:
			doctors.availableDoctors();
			break;
		case 4:
			doctors.Assign();
			break;
		case 5:
			doctors.updateList();
			break;
		case 6:
			doctors.delete_by_id();
			break;
		case 7:
			cout << "Returning to main menu...\n";
			break;
		default:
			cout << "Invalid choice. Try again.\n";
		}
	} while (choice != 7);
}

void displayAdminMenu(patientList& patients, doctorList& doctors, appointmentQueue& appointments, MedicationStack& medications, Billing& billing) {
	int choice;
	do {
		cout << "\n--- Admin Menu ---\n";
		cout << "1. Display All Patients\n";
		cout << "2. Display All Doctors\n";
		cout << "3. Schedule an Appointment\n";
		cout << "4. Display Appointments\n";
		cout << "5. Add Medication for a Patient\n";
		cout << "6. Display Medication History\n";
		cout << "7. Generate a Bill\n";
		cout << "8. Go Back\n";
		cout << "Enter your choice: ";
		cin >> choice;

		switch (choice) {
		case 1:
			patients.display();
			break;
		case 2:
			doctors.display();
			break;
		case 3: {
			int patientID, doctorID;
			string time;
			cout << "Enter Patient ID: ";
			cin >> patientID;
			cout << "Enter Doctor ID: ";
			cin >> doctorID;
			cout << "Enter Appointment Time: ";
			cin >> time;
			appointments.enqueue(patientID, doctorID, time);
			break;
		}
		case 4:
			appointments.displayAppointments();
			break;
		case 5: {
			int patientID;
			string medName, dosage;
			cout << "Enter Patient ID: ";
			cin >> patientID;
			cout << "Enter Medication Name: ";
			cin >> medName;
			cout << "Enter Dosage: ";
			cin >> dosage;
			medications.push(patientID, medName, dosage);
			break;
		}
		case 6:
			medications.displayMedications();
			break;
		case 7: {
			int patientID, daysAdmitted, medsCost;
			cout << "Enter Patient ID: ";
			cin >> patientID;
			cout << "Enter Days Admitted: ";
			cin >> daysAdmitted;
			cout << "Enter Medication Cost: ";
			cin >> medsCost;
			billing.displayBill(patientID, daysAdmitted, medsCost);
			break;
		}
		case 8:
			cout << "Returning to main menu...\n";
			break;
		default:
			cout << "Invalid choice. Try again.\n";
		}
	} while (choice != 8);
}

void mainMenu() {
	patientList patients;
	doctorList doctors;
	appointmentQueue appointments;
	MedicationStack medications;
	Billing billing;
	patients.loadFile();
	doctors.loadFile();
	int role;
	do {
		cout << "\nWELCOME TO HOSPITAL MANAGEMENT SYSTEM\n";
		cout << "\n--- Main Menu ---\n";
		cout << "1. Patient\n";
		cout << "2. Doctor\n";
		cout << "3. Admin\n";
		cout << "4. Exit\n";
		cout << "Enter your role: ";
		cin >> role;

		switch (role) {
		case 1:
			displayPatientMenu(patients);
			break;
		case 2:
			displayDoctorMenu(doctors);
			break;
		case 3:
			displayAdminMenu(patients, doctors, appointments, medications, billing);
			break;
		case 4:
			cout << "Exiting the program. Goodbye!\n";
			break;
		default:
			cout << "Invalid role. Please try again.\n";
		}
	} while (role != 4);
}

int main() {
	mainMenu();
	return 0;
}
