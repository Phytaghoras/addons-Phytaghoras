Berikut klasifikasi lengkap seluruh kode di **hotel_management_odoo** dan **hotel_reservation_dashboard**, sesuai permintaan kamu.

---

#### Tabel Ringkas Klasifikasi

| Fungsi                   | Modul                          | Kode Utama                                                                                             | Tipe Kode                            |
|--------------------------|--------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------|
| Menu Structure           | hotel_management_odoo          | `views/hotel_menu.xml`                                                                                 | XML: `<menuitem>` & `<act_window>`   |
| Room Management          | hotel_management_odoo          | `models/hotel_room.py`<br>`views/hotel_room_views.xml`<br>`security/ir.model.access.csv`               | Python Model;<br>XML Views;<br>ACL   |
| Room Booking             | hotel_management_odoo          | `models/room_booking.py`<br>`views/room_booking_views.xml`<br>`data/sequences.xml`<br>`wizard/intervene_booking.py` | Python Model;<br>XML Views;<br>Data;<br>Wizard |
| Housekeeping             | hotel_management_odoo          | `models/cleaning_request.py`<br>`models/cleaning_team.py`<br>`views/cleaning_views.xml`                 | Python Model;<br>XML Views           |
| Event Space Booking      | hotel_management_odoo          | `models/event_booking_line.py`<br>`views/event_views.xml`                                               | Python Model;<br>XML Views           |
| Fleet Reservation        | hotel_management_odoo          | `models/fleet_booking_line.py`<br>`views/fleet_views.xml`                                               | Python Model;<br>XML Views           |
| Reports & PDF Templates  | hotel_management_odoo          | `report/*.xml`<br>`views/report_views.xml`                                                              | QWeb/RML Templates;<br>XML Views     |
| Dashboard                | hotel_management_odoo &<br>hotel_reservation_dashboard | `hotel_management_odoo/views/dashboard.xml`<br>`hotel_reservation_dashboard/views/dsm_menu.xml`<br>`static/src/js/dsm.js`<br>`static/src/xml/dsm.xml`<br>`static/src/css/dsm.css` | XML Views;<br>JS Widget;<br>Static Assets |
| Security & ACL           | hotel_management_odoo          | `security/groups.xml`<br>`security/ir.model.access.csv`                                                 | XML Groups;<br>CSV ACL               |
| HTTP Controllers         | hotel_management_odoo          | `controllers/*.py`                                                                                      | Python HTTP Endpoints                |
| Wizards                  | hotel_management_odoo          | `wizard/*.py`<br>`views/*_wizard.xml`                                                                  | Python Wizards;<br>XML Views         |
| Data & Initial Setup     | hotel_management_odoo          | `data/sequences.xml`<br>`data/*.xml`                                                                    | XML Data Records                     |

---

## Penjelasan Lengkap

### 1. Menu Structure  
- **File**: `hotel_management_odoo/views/hotel_menu.xml`  
- **Isi & Fungsi**:   
  - Mendefinisikan top-level menu “Hotel” (`<menuitem id="hotel_menu_root" name="Hotel"/>`)  
  - Sub-menu: Rooms, Bookings, Cleaning, Events, Fleet Reservations, Reports, Dashboard  
  - Mengaitkan setiap menu ke `action_window` yang menunjuk ke view tertentu  

### 2. Room Management  
- **Model**: `hotel_management_odoo/models/hotel_room.py`  
  - Kelas `HotelRoom(models.Model)` dengan field `name`, `room_type`, `capacity`, `amenities`  
  - Constraint Python (mis. kapasitas > 0)  
- **Views**: `hotel_management_odoo/views/hotel_room_views.xml`  
  - Form view (`<record id="view_hotel_room_form" model="ir.ui.view">…</record>`)  
  - Tree & kanban view untuk daftar kamar  
  - Calendar view berdasarkan `availability_date`  
- **Access Control**: `hotel_management_odoo/security/ir.model.access.csv`  
  - Baris untuk `hotel.room` memberikan `read/write` kepada `group_hotel_manager`  

### 3. Room Booking  
- **Model**: `hotel_management_odoo/models/room_booking.py`  
  - Kelas `RoomBooking(models.Model)` dengan field `room_id`, `check_in`, `check_out`, `state`  
  - Method `action_confirm()`, `action_cancel()` untuk lifecycle booking  
- **Views**: `hotel_management_odoo/views/room_booking_views.xml`  
  - Form view (`view_room_booking_form`) menampilkan tanggal, tamu, status  
  - Tree & Calendar view  
  - Menu “Bookings” mengacu ke action ini  
- **Data**: `hotel_management_odoo/data/sequences.xml`  
  - Mendefinisikan sequence `hotel.booking.sequence` untuk nomor reservasi  
- **Wizard**:   
  - `hotel_management_odoo/wizard/intervene_booking.py` (Python)  
  - View wizard `views/intervene_booking_wizard.xml` untuk modifikasi cepat  

### 4. Housekeeping (Cleaning)  
- **Models**:   
  - `models/cleaning_request.py`: request pembersihan per kamar, due date, assigned_to  
  - `models/cleaning_team.py`: definisi tim, anggota, capacity per shift  
- **Views**: `views/cleaning_views.xml`  
  - List view + Calendar view untuk jadwal pembersihan  
  - Form view untuk membuat/menugaskan cleaning request  
- **Menu**: di bawah “Hotel” → “Cleaning Tasks”  

### 5. Event Space Booking  
- **Model**: `hotel_management_odoo/models/event_booking_line.py`  
  - Menyimpan pemesanan ruang acara: event_id, start/end, peserta  
- **Views**: `views/event_views.xml`  
  - Form, Tree & Calendar view  
- **Menu**: di bawah “Hotel” → “Events”  

### 6. Fleet Reservation  
- **Model**: `hotel_management_odoo/models/fleet_booking_line.py`  
  - Booking kendaraan: vehicle_id, start/end, biaya  
- **Views**: `views/fleet_views.xml`  
  - Form & Tree view  
- **Menu**: di bawah “Hotel” → “Fleet Reservations”  

### 7. Reports & PDF Templates  
- **Files**: di `hotel_management_odoo/report/`  
  - `booking_confirmation_templates.xml`: QWeb template untuk konfirmasi booking  
  - `invoice_templates.xml`: template faktur extended dengan line item kamar/layanan  
  - `cleaning_report_templates.xml`: laporan pembersihan  
- **Actions**:   
  - Didefinisikan di `views/report_views.xml` sebagai `<report>` yang menunjuk ke template  

### 8. Dashboard  
- **Core Dashboard** (`hotel_management_odoo`)  
  - `views/dashboard.xml`: page dashboard sederhana (statistik occupancy)  
- **Reservation Dashboard** (`hotel_reservation_dashboard`)  
  - **Menu**: `views/dsm_menu.xml` menambahkan “Reservation Dashboard”  
  - **Widget JS**: `static/src/js/dsm.js`  
    - Menggunakan Odoo RPC untuk fetch data (occupancy, revenue, forecast)  
  - **Templates**: `static/src/xml/dsm.xml` (client-side QWeb templates)  
  - **Styles**: `static/src/css/dsm.css` dan DatePicker assets via CDN  
  - **Keamanan**: `security/dashboard_security.xml` (commented out) untuk membatasi ke `group_hotel_manager`  

### 9. Security & Access Control  
- **Groups**: `security/groups.xml`  
  - `group_hotel_user`: hanya lihat data  
  - `group_hotel_manager`: buat/ubah/hapus data  
- **Model ACL**: `security/ir.model.access.csv`  
  - Rinciannya untuk setiap model (`hotel.room`, `room.booking`, dsb)  

### 10. HTTP Controllers  
- **Files**: `controllers/main.py` (atau beberapa file dalam `controllers/`)  
  - Route seperti `/hotel/book` untuk portal eksternal (auth="public")  
  - Endpoint AJAX `/hotel/dashboard/data` (auth="user")  

### 11. Data & Initial Setup  
- **Sequences**: `data/sequences.xml`  
- **Mail Templates**: mis. `data/mail_template_booking.xml` untuk email konfirmasi  

### 12. Wizards  
- **Python**: `wizard/intervene_booking.py`, `wizard/generate_booking_sheet.py`  
- **Views**: wizard definitions di `views/intervene_booking_wizard.xml`, dsb
