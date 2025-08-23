# Card Sequence Validator - Project Description

## Table of Contents
1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [System Architecture](#system-architecture)
4. [Component Descriptions](#component-descriptions)
   - [GUI Layer](#gui-layer)
   - [Logic Layer](#logic-layer)
   - [Service Layer](#service-layer)
   - [Utility Components](#utility-components)
5. [Data Flow](#data-flow)
6. [Features and Functionality](#features-and-functionality)
7. [Setup and Usage](#setup-and-usage)

## Project Overview

The Card Sequence Validator is a Python-based desktop application designed for the card manufacturing industry. Its primary purpose is to ensure the correct sequence of manufactured cards by scanning QR codes via a COM port interface and validating them against a preloaded sequence file.

The application provides real-time feedback on the scanning process, automatically handles mismatches by searching for the scanned card in the remaining sequence, and maintains detailed logs for quality assurance and audit purposes. This solution significantly improves quality control in card manufacturing by automating the sequence validation process and reducing human error.

## Technology Stack

### Core Technologies
- **Python 3.x**: Primary programming language chosen for its simplicity, extensive libraries, and cross-platform compatibility
- **PyQt6/PyQt5**: GUI framework providing a modern, cross-platform user interface with native look and feel
- **pyserial**: Library for serial communication, enabling connection to COM ports for QR scanner integration

### Supporting Libraries
- **numpy**: Numerical computing library used for efficient data processing
- **pandas**: Data manipulation library for handling structured data in various formats
- **pillow (PIL)**: Python Imaging Library for handling image assets
- **python-dateutil**: Extensions to the standard datetime module for enhanced date/time handling
- **pytz**: World timezone definitions for accurate timestamp management
- **six**: Python 2 and 3 compatibility library
- **tzdata**: Timezone data for accurate time calculations

## System Architecture

The application follows a layered architecture pattern with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────┐
│                      Presentation Layer                     │
│                      (GUI Components)                       │
├─────────────────────────────────────────────────────────────┤
│                        Service Layer                        │
│              (Business Logic & Data Processing)             │
├─────────────────────────────────────────────────────────────┤
│                        Logic Layer                          │
│                (Communication & File Handling)              │
└─────────────────────────────────────────────────────────────┘
```

This architecture ensures:
- Modularity and maintainability
- Clear separation of concerns
- Reusability of components
- Testability of individual modules

## Component Descriptions

### GUI Layer

#### `gui/main.py`
This is the main application file containing the `ModernCardValidator` class, which serves as the primary window of the application. Key features include:

- **User Interface Management**: Comprehensive UI with styled components using PyQt6
- **COM Port Handling**: Integration with COM port reader for real-time data acquisition
- **File Operations**: Loading sequence files, previewing content, and downloading logs
- **Log Management**: Displaying scan results in a color-coded table with timestamp information
- **Card Display**: Showing current and expected card information
- **State Management**: Handling application state including start/stop functionality

The UI is styled with a custom stylesheet that provides a modern, professional appearance with:
- Color-coded status indicators (green for OK, red for NOT OK)
- Responsive layout with appropriate spacing and sizing
- Consistent styling across all components

#### `gui/ui/preview_window.py`
A dialog component that displays the expected card sequence in a tabular format. Features include:
- Non-editable table showing NUMCARD and ICCID pairs
- Responsive column sizing for better readability
- Inherited styling from the main application for consistency

#### `gui/ui/select_start_card_dialog.py`
A dialog that allows users to select a starting point in the card sequence. Features include:
- List view of all available cards with NUMCARD and ICCID information
- Simple selection mechanism with OK/Cancel buttons
- Integration with the main application's styling

### Logic Layer

#### `logic/com_reader.py`
Handles all aspects of COM port communication:
- **Serial Connection Management**: Establishes and maintains connection to the QR scanner
- **Threading**: Uses background threads to prevent UI blocking during data acquisition
- **Data Processing**: Decodes raw serial data and filters out non-printable characters
- **Error Handling**: Comprehensive error handling for serial communication issues
- **Callback System**: Uses callback functions to communicate with higher layers

Key features:
- Configurable baud rate and serial parameters
- Real-time data reading with timeout handling
- Robust error reporting to the UI layer

#### `logic/com_selector.py`
Simple utility module that lists available COM ports on the system:
- Uses `serial.tools.list_ports` to enumerate available ports
- Returns a list of port identifiers for UI display

#### `logic/file_parser.py`
Acts as a router for file parsing operations:
- Determines appropriate parser based on file extension
- Supports .cpd, .txt, and .csv file formats
- Returns structured data for validation

### Service Layer

#### `services/card_validator.py`
Contains the core validation logic:
- **Data Processing**: Handles incoming scanned data from the COM port
- **Sequence Validation**: Compares scanned codes with expected values
- **Mismatch Handling**: Implements intelligent search for scanned cards in the remaining sequence
- **Log Management**: Adds entries to the application log with timestamp and status information
- **UI Updates**: Communicates status updates to the user interface

Key features:
- Automatic sequence adjustment when mismatches occur
- Detailed logging of all scan events including skipped cards
- Status bar updates for real-time feedback

#### `services/file_service.py`
Implements parsing for different file formats:
- **CPD File Parsing**: Extracts NUMCARD and ICCID information from CPD files
- **Text File Parsing**: Handles simple text files containing ICCID data
- **CSV File Parsing**: Processes CSV files with NUMCARD and ICCID columns
- **Data Structuring**: Returns data in a consistent format for validation

Each parser:
- Handles file reading with proper encoding
- Extracts relevant data based on file format
- Structures data as pairs of (current_card, next_card) for validation

### Utility Components

#### `com_simulator.py`
A standalone utility for testing the application without physical hardware:
- **File-based Simulation**: Sends data from a text file line by line
- **Interactive Mode**: Allows manual input of test data
- **Configurable Timing**: Adjustable delays between data transmissions
- **Error Handling**: Comprehensive error handling for file and serial issues

This tool is invaluable for:
- Development and testing without hardware
- Demonstrating application functionality
- Troubleshooting communication issues

#### `constants.py`
Centralized configuration management:
- UI labels and messages
- File paths and filters
- Status messages and error texts
- Application titles and identifiers

This approach ensures:
- Consistent labeling throughout the application
- Easy localization and customization
- Centralized management of application parameters

## Data Flow

1. **Application Initialization**:
   - Main window is created with all UI components
   - COM ports are enumerated and displayed
   - Application waits for user input

2. **File Loading**:
   - User selects a sequence file (.cpd, .txt, or .csv)
   - File parser routes to appropriate parser based on extension
   - File service parses the file and structures data
   - Expected card sequence is loaded into memory

3. **COM Port Communication**:
   - User selects a COM port and starts reading
   - COM reader establishes connection to the selected port
   - Background thread continuously monitors for incoming data
   - Raw data is decoded and cleaned

4. **Validation Process**:
   - Scanned data is passed to the card validator
   - Validator compares scanned code with expected value
   - If match: Log entry is created with "OK" status
   - If mismatch: Intelligent search for card in remaining sequence
   - Sequence index is updated accordingly

5. **User Feedback**:
   - Status bar shows current operation status
   - Log table displays all scan events with color-coded status
   - Current and next expected cards are displayed
   - Visual feedback for successful or failed scans

6. **Logging**:
   - All scan events are logged with timestamps
   - Logs can be downloaded as CSV files
   - Log data persists in memory for export

## Features and Functionality

### Core Features
- **Real-time Card Scanning**: Continuous monitoring of COM port for QR code data
- **Sequence Validation**: Automatic verification of card sequence against loaded file
- **Intelligent Mismatch Handling**: Automatic search for scanned cards in remaining sequence
- **Comprehensive Logging**: Detailed logs with timestamps for audit purposes
- **Multiple File Format Support**: Support for CPD, TXT, and CSV files
- **Visual Feedback**: Color-coded status indicators for immediate feedback

### User Interface Features
- **Modern UI Design**: Professional appearance with consistent styling
- **COM Port Management**: Easy selection and monitoring of serial ports
- **File Operations**: Simple file loading, previewing, and clearing
- **Log Management**: Tabular display of scan results with export capability
- **Card Display**: Clear presentation of current and expected card information
- **Start Card Selection**: Ability to begin processing from any point in sequence

### Advanced Features
- **Sequence Resumption**: Ability to continue from a specific card in the sequence
- **Skipped Card Logging**: Automatic logging of skipped cards when mismatches occur
- **Status Monitoring**: Real-time status updates in the status bar
- **Clock Display**: Current timestamp display for reference
- **Error Handling**: Comprehensive error handling with user-friendly messages

## Setup and Usage

### Prerequisites
- Python 3.7 or higher
- Required Python packages (listed in requirements.txt)

### Installation
1. Clone or download the repository
2. Install required packages:
   ```bash
   pip install -r requirements.txt
   ```

### Running the Application
1. Launch the main application:
   ```bash
   python gui/main.py
   ```

### Using the COM Simulator (for testing)
1. Run the simulator in interactive mode:
   ```bash
   python com_simulator.py
   ```
2. Or run with a data file:
   ```bash
   python com_simulator.py -f data.txt -d 0.5
   ```

### Building an Executable
To create a standalone executable:
```bash
pyinstaller --onefile gui/main.py
```

### Basic Usage Workflow
1. Select a COM port from the dropdown
2. Load a sequence file (.cpd, .txt, or .csv)
3. Click "Start" to begin scanning
4. Scan cards using the connected QR scanner
5. Monitor results in the log table
6. Download logs when needed

This comprehensive solution provides an efficient and reliable method for validating card sequences in manufacturing environments, with robust error handling and detailed logging for quality assurance purposes.