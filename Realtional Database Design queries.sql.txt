-- =========================================================================
PROJECT: RELATIONAL DATABASE DESIGN & COMPLEX QUERY ANALYTICS
-- Database Management System: PostgreSQL
-- =========================================================================

-- =========================================================================
-- SECTION 1: COMPANY DATABASE ANALYTICS (Exercise 8.16)
-- =========================================================================

-- Soal A: Nama karyawan di departemen 5 yang bekerja > 10 jam per minggu pada proyek 'ProductX'
SELECT 
    e.fname, 
    e.lname 
FROM employee e
INNER JOIN works_on w ON e.ssn = w.essn
INNER JOIN project p ON w.pno = p.pnumber
WHERE e.dno = 5 
  AND w.hours > 10 
  AND p.pname = 'ProductX';


-- Soal B: Daftar nama karyawan yang memiliki nama depan yang sama dengan tanggungannya
SELECT 
    e.fname, 
    e.lname 
FROM employee e
INNER JOIN dependent d ON e.ssn = d.essn
WHERE e.fname = d.dependent_name;


-- Soal C: Nama-nama karyawan yang diawasi langsung oleh 'Franklin Wong'
SELECT 
    es.fname, 
    es.lname
FROM employee e
INNER JOIN employee es ON e.ssn = es.super_ssn
WHERE e.fname = 'Franklin' 
  AND e.lname = 'Wong';


-- Soal D: Untuk setiap proyek, tampilkan nama proyek dan total jam kerja per minggu dari semua karyawan
SELECT 
    p.pname, 
    SUM(w.hours) AS total_hours
FROM project p
INNER JOIN works_on w ON p.pnumber = w.pno
GROUP BY p.pname;


-- Soal E: Nama karyawan yang bekerja di semua proyek perusahaan (Relational Division)
SELECT 
    e.fname, 
    e.lname
FROM employee e
WHERE NOT EXISTS (
    SELECT p.pnumber 
    FROM project p
    WHERE NOT EXISTS (
        SELECT * 
        FROM works_on w
        WHERE w.essn = e.ssn 
          AND w.pno = p.pnumber
    )
);


-- Soal F: Nama karyawan yang sama sekali tidak terlibat dalam proyek apa pun
SELECT 
    fname, 
    lname
FROM employee
WHERE ssn NOT IN (
    SELECT essn 
    FROM works_on
);


-- Soal G: Nama departemen beserta rata-rata gaji karyawan yang bekerja di departemen tersebut
SELECT 
    d.dname, 
    AVG(e.salary) AS average_salary
FROM department d
INNER JOIN employee e ON d.dnumber = e.dno
GROUP BY d.dname;


-- Soal H: Rata-rata gaji seluruh karyawan perempuan (Female)
SELECT 
    AVG(salary) AS avg_female_salary
FROM employee
WHERE sex = 'F';


-- Soal I: Nama dan alamat karyawan yang bekerja pada minimal satu proyek di Houston, 
-- tetapi departemennya sendiri tidak berlokasi di Houston
SELECT DISTINCT 
    e.fname, 
    e.lname, 
    e.address
FROM employee e
INNER JOIN works_on w ON e.ssn = w.essn
INNER JOIN project p ON w.pno = p.pnumber
INNER JOIN department d ON e.dno = d.dnumber
INNER JOIN dep_locations dl ON d.dnumber = dl.dnumber
WHERE p.plocation = 'Houston' 
  AND dl.dlocation != 'Houston';


-- Soal J: Nama belakang manajer departemen yang tidak memiliki tanggungan sama sekali
SELECT 
    e.lname
FROM employee e
INNER JOIN department d ON e.ssn = d.mgr_ssn
WHERE NOT EXISTS (
    SELECT * 
    FROM dependent dp
    WHERE e.ssn = dp.essn
);


-- =========================================================================
-- SECTION 2: LIBRARY LOAN DATABASE ANALYTICS (Exercise 8.18)
-- =========================================================================

-- Soal A: Jumlah salinan buku 'The Lost Tribe' yang dimiliki oleh cabang perpustakaan 'Sharpstown'
SELECT 
    lb.branch_name,
    bc.no_of_copies
FROM library_branch lb
INNER JOIN book_copies bc ON lb.branch_id = bc.branch_id
INNER JOIN book b ON bc.book_id = b.book_id
WHERE lb.branch_name = 'Sharpstown'
  AND b.title = 'The Lost Tribe';


-- Soal B: Jumlah salinan buku 'The Lost Tribe' yang dimiliki oleh masing-masing cabang perpustakaan
SELECT 
    lb.branch_name,
    bc.no_of_copies
FROM library_branch lb
INNER JOIN book_copies bc ON lb.branch_id = bc.branch_id
INNER JOIN book b ON bc.book_id = b.book_id
WHERE b.title = 'The Lost Tribe';


-- Soal C: Nama semua peminjam yang saat ini sedang tidak meminjam buku apa pun (tidak ada pinjaman aktif)
SELECT 
    b.name, 
    b.address
FROM borrower b
LEFT JOIN book_loans bl ON b.card_no = bl.card_no
WHERE bl.card_no IS NULL;


-- Soal D: Judul buku, nama peminjam, dan alamat peminjam untuk setiap buku yang dipinjam 
-- dari cabang 'Sharpstown' dengan tanggal jatuh tempo hari ini
SELECT 
    b.title, 
    br.name AS borrower_name, 
    br.address AS borrower_address
FROM book_loans bl
INNER JOIN book b ON bl.book_id = b.book_id
INNER JOIN borrower br ON bl.card_no = br.card_no
INNER JOIN library_branch lb ON bl.branch_id = lb.branch_id
WHERE lb.branch_name = 'Sharpstown'
  AND bl.due_date = CURRENT_DATE; -- Menggunakan fungsi bawaan PostgreSQL untuk 'Today'


-- Soal E: Total jumlah buku yang dipinjam dari masing-masing cabang perpustakaan
SELECT 
    lb.branch_name,
    COUNT(bl.book_id) AS total_books_loaned
FROM library_branch lb
INNER JOIN book_loans bl ON lb.branch_id = bl.branch_id
GROUP BY lb.branch_name;


-- Soal F: Nama, alamat, dan jumlah buku yang dipinjam untuk peminjam yang meminjam lebih dari 5 buku
SELECT 
    b.name, 
    b.address,
    COUNT(bl.book_id) AS total_books_borrowed
FROM borrower b
INNER JOIN book_loans bl ON b.card_no = bl.card_no
GROUP BY b.name, b.address
HAVING COUNT(bl.book_id) > 5;


-- Soal G: Nama cabang, judul buku, dan jumlah salinan untuk setiap buku yang ditulis oleh 'Stephen King' 
-- di cabang perpustakaan 'Central'
SELECT 
    lb.branch_name, 
    b.title,
    bc.no_of_copies
FROM library_branch lb
INNER JOIN book_copies bc ON lb.branch_id = bc.branch_id
INNER JOIN book b ON bc.book_id = b.book_id
INNER JOIN book_authors ba ON b.book_id = ba.book_id
WHERE lb.branch_name = 'Central'
  AND ba.author_name = 'Stephen King';