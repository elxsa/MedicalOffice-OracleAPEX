
As part of this project, I used Oracle APEX to create an application for managing a medical office.
- The Homepage contains buttons that redirect the application to each respective page.

![Screenshot_16](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/8326e236-1440-48ff-a955-24a115e5e068)

- A database was created with the following tables:

![tabelemedic](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/ac29a8bb-7386-4d8d-ac07-c3437eb274d7)


- The "Vizualizare medici" (EN: Doctors view) page contains all the doctors from the database and is a Classic Report page. On this page, we can see all the details about the doctors in the medical office.

![Screenshot_17](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/0a7056a5-05de-41fd-9ccc-143fc9e73b3c)

- The "Pacienții unui medic" (EN: Patients view) page is a Master Detail page, where the master table is represented by the Doctors table, and the detail table is represented by the Appointments table, thus obtaining the appointments for the selected doctor.

![Screenshot_18](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/a2d30d6d-9afe-4430-b756-97e7c76177aa)

- On this "Vizualizare programări" (EN: Appointments view) page for a patient, we have a Page Item CNP, which takes the entered CNP and returns the appointments for the patient with that CNP.

![Screenshot_19](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/8dd55ffa-b928-4071-8c0b-8a845871a8fa)

For this purpose, I used the following SQL query:
SELECT 
    p.codPro,
    p.dataP,
    p.nume,
    p.medID,
    p.cabinet
FROM 
    cbProgramare p
JOIN 
    cbReteta r ON p.cnp = r.cnp
WHERE 
    r.cnp LIKE :P_CNP;

where P1_CNP is the Page Item CNP, as the CNP is the primary key in the patients table.

- The "Raport medici" (EN: Doctors report) page:

![Screenshot_20](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/ed6b2b44-65fc-41c5-97a5-2dce8998c17d)

To count the appointments each doctor has, I used the following query:
SELECT 
    m.nume AS doctor_name,
    s.denumire AS specialty,
    g.denumire AS degree,
    COUNT(p.codPro) AS appointment_count
FROM 
    cbMedic m
INNER JOIN 
    cbProgramare p ON m.medID = p.medID
INNER JOIN 
    cbSpecialitate s ON m.codSpec = s.codSpec
INNER JOIN 
    cbGrad g ON m.codGrad = g.codGrad
GROUP BY 
    m.nume, s.denumire, g.denumire;

This query retrieves the doctor's name, their specialty, their degree, and the number of appointments they have.

- This page is the "Adăugare programare" (EN: Add appointment) page, where the CNP and Doctor ID, being primary keys in other tables, should ideally be auto-completed:

![Screenshot_21](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/099d6763-c9ad-4fc6-ad14-a294217632a2)

- The "Raport specializări" (EN: Statistics report for specialties) page:

![Screenshot_21](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/fb35e63e-c26d-4c89-9585-7fa77361cc8c)

For reporting the statistics of the specialties, I used the following query:

SELECT 
    sp.codSpec,
    sp.denumire AS denumire_specialitate,
    COUNT(DISTINCT m.medID) AS numar_total_medici,
    COUNT(DISTINCT pr.cnp) AS numar_total_pacienti
FROM 
    cbSpecialitate sp
LEFT JOIN 
    cbMedic m ON sp.codSpec = m.codSpec
LEFT JOIN 
    cbProgramare pr ON m.medID = pr.medID
GROUP BY 
    sp.codSpec, sp.denumire;

to count the total number of patients and the total number of doctors by specialties.

- In the context of developing the application for the medical office, using a database schema had several essential roles: organizing data, isolating and securing data, avoiding naming conflicts, facilitating management and maintenance, and improving performance.

![schemabd](https://github.com/elxsa/MedicalOffice-OracleAPEX/assets/146994240/d10474b0-9de3-43a5-bc2e-452c10aef017)

