
As part of this project, I used Oracle APEX to create an application for managing a medical office.
- The Homepage contains buttons that redirect the application to each respective page.

![Screenshot_16](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/8326e236-1440-48ff-a955-24a115e5e068)

- A database was created with the following tables:

![tabelemedic](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/ac29a8bb-7386-4d8d-ac07-c3437eb274d7)


The "Vizualizare medici" (EN: Doctors view) page contains all the doctors from the database and is a Classic Report page. On this page, we can see all the details about the doctors in the medical office.

![Screenshot_17](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/0a7056a5-05de-41fd-9ccc-143fc9e73b3c)

The "Pacienții unui medic" (EN: Patients view) page is a Master Detail page, where the master table is represented by the Doctors table, and the detail table is represented by the Appointments table, thus obtaining the appointments for the selected doctor.

![Screenshot_18](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/a2d30d6d-9afe-4430-b756-97e7c76177aa)

On this "Vizualizare programări" (EN: Appointments view) page for a patient, we have a Page Item CNP, which takes the entered CNP and returns the appointments for the patient with that CNP.

![Screenshot_19](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/8dd55ffa-b928-4071-8c0b-8a845871a8fa)

For this purpose, I used the following SQL query:
SELECT p.codPro, p.dataP, p.nume, p.medID, p.cabinet
FROM cbProgramare p
JOIN cbReteta r ON p.cnp = r.cnp
WHERE r.cnp like :P_CNP;

where P1_CNP is the Page Item CNP, as the CNP is the primary key in the patients table.






