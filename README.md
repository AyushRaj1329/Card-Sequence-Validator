# 🖥️ Card Sequence Validator

A Python-based desktop GUI application designed for the card manufacturing industry. It scans cards in real-time using a QR scanner via the COM port, verifies their sequence from a preloaded file (.cpd, .txt, or .csv), and provides immediate feedback. Logs are auto-generated for audit and validation purposes.

---

## 📁 Folder Structure

```bash
card-sequence-validator/
│
├── assets/                      # Visual assets
│   └── logo.png                 # Company or app logo shown in the GUI
│
├── gui/                         # All GUI-related files
│   ├── main.py                  # Main GUI application and entry point
│   └── ui/                      # UI dialog components
│       ├── preview_window.py    # Preview window for expected cards
│       └── select_start_card_dialog.py  # Dialog for selecting start card
│
├── logic/                       # Core processing and backend logic
│   ├── com_reader.py            # Reads input from the serial (COM) port
│   ├── com_selector.py          # Lists available COM ports
│   └── file_parser.py           # Routes to appropriate file parser
│
├── services/                    # Service layer components
│   ├── card_validator.py        # Card validation logic
│   └── file_service.py          # File parsing implementations
│
├── com_simulator.py             # COM port simulator for testing
├── constants.py                 # Application constants
├── README.md                    # Complete project documentation and setup guide
├── requirements.txt             # All required Python libraries for the app
└── .gitignore                   # Files and folders ignored by Git
```

---

## 🧰 Tech Stack

| Technology     | Purpose                            |
|----------------|-------------------------------------|
| Python         | Core programming language           |
| PyQt6/PyQt5    | GUI framework                       |
| pyserial       | Reading data from the COM port      |
| csv / os       | File handling and system I/O        |

---

## 🚀 Features

- Upload `.cpd`, `.txt`, or `.csv` files for card sequence validation
- Real-time QR code scan input via COM port
- Display current scan and expected value
- Visual feedback: ✅ OK / ❌ NOT OK
- Option to select starting card for processing
- Log generation in `.csv` format
- Preview expected card sequence within GUI
- Mismatch handling with auto-resume search feature
- Desktop executable ready (via `pyinstaller`)

---

## 📦 Setup Instructions

```bash
git clone <repository-url>
cd card-sequence-validator
pip install -r requirements.txt
python gui/main.py
```

To build into an executable:

```bash
pyinstaller --onefile gui/main.py
```

---

## 📌 Future Enhancements

- Download button for logs
- Error reports by date
- Export in multiple formats (CSV, XLSX)
- Multi-user authentication

---

## 📄 License

This project is licensed under the MIT License.

---

> 💡 *Built for quality assurance and sequence validation in card manufacturing.*