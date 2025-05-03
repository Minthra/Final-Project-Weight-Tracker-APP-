# Weight Tracker System

The **Weight Tracker System** is a Windows Forms application I developed using **C# and .NET 8.0** to help users track workouts, manage exercises, and visualize progress. I implemented user authentication, workout logging, progress charts, and custom exercise creation, using **SQLite** for data storage. My work ensures robust functionality, such as fixing the issue where custom exercises didn’t appear in the exercise list, and applies **object-oriented programming (OOP)** principles for a modular design.

---

## Prerequisites

- **Operating System:** Windows 10 or later  
- **.NET 8.0 Runtime:** Required to run the application  
- **Visual Studio 2022:** For opening and running the project  
- **Database:** `weight_tracker.db` (included in the project folder)  
- **Dependencies:** `System.Data.SQLite.Core v1.0.118.0` (included in the project)

---

## My Contributions

I designed and implemented the core functionality of the Weight Tracker System, focusing on the following components:

### `DatabaseHelper.cs`

**What I Did:**  
- Created a database management class to handle all SQLite operations.
- Defined the `IDatabaseService` interface.
- Implemented methods for user authentication, exercise retrieval, workout logging, and custom exercise creation.
- Added support for `equipment_type` and `plate_weights` in the `exercises` and `workouts` tables.
- Ensured data integrity with foreign keys and included detailed debug logging.

**Key Features:**
- Initializes `users`, `exercises`, and `workouts` tables with schema.
- Methods: `AuthenticateUser`, `RegisterUser`, `GetExercises`, `AddCustomExercise`, `LogWorkout`, `GetChartData`
- Fixed database errors by handling SQLite constraints (e.g., duplicate exercise names).

---

### `CustomExerciseForm.cs`

**What I Did:**  
- Developed a form to add custom exercises with name and equipment type (Barbell/Dumbbell).
- Implemented input validation using Regex and checks for duplicates.
- Fixed saving issues by enhancing error handling in `saveButton_Click`.

**Key Features:**
- Validates input (letters, numbers, spaces, max 50 characters).
- Saves exercises via `AddCustomExercise` with success/error messages.
- Navigates back to `ExerciseSelectionForm`.

---

### `ExerciseSelectionForm.cs`

**What I Did:**  
- Created a form to select or add custom exercises.
- Fixed the critical bug where custom exercises didn’t appear in the list by adding `RefreshExerciseList` in `OnShown`.
- Implemented navigation to `WorkoutInputForm` and `CustomExerciseForm`.

**Key Features:**
- Populates ComboBox using `GetExercises`, refreshed on form show.
- Handles selection, custom exercise addition, and back navigation.
- Displays errors if no exercises are available.

---

### `ProgressChartForm.cs`

**What I Did:**  
- Built a form to visualize workout progress as a line chart.
- Implemented dynamic chart rendering using `PictureBox`.
- Fixed issues with incorrect column references and prevented date label overlap.
- Ensured support for custom exercises and kg/lbs units.

**Key Features:**
- Fetches data with `GetChartData`, plots weight vs. date.
- Handles empty/invalid data gracefully.
- Updates chart on exercise/unit change.

---

### Other Forms

- **`LoginForm.cs`**: Implemented user login and registration with secure password hashing.
- **`WorkoutInputForm.cs`**: Created workout logging with equipment type and plate weights.
- **`MainMenuForm.cs`, `SplashForm.cs`, `WorkoutHistoryForm.cs`, `RegistrationForm.cs`**: Developed for navigation and extra features.

---

## OOP Concepts

### Encapsulation
- Used private fields (e.g., `DatabaseHelper.connectionString`) with controlled public access.
- Example: `ProgressChartForm`'s `chartData` is private and updated only by `UpdateChart`.

### Inheritance
- All forms derive from `System.Windows.Forms.Form`.
- Example: `ExerciseSelectionForm` overrides `OnShown` to refresh exercises.

### Polymorphism
- Overrode methods like `OnFormClosing` for navigation logic.
- Example: `ChartPictureBox_Paint` implements custom chart rendering.

### Abstraction
- Defined `IDatabaseService` to abstract SQLite details.
- Example: Forms call `db.AddCustomExercise` without DB logic.

### Single Responsibility Principle (SRP)
- Each class/method does one thing well.
- Example: `RefreshExerciseList` only updates the ComboBox.

### Dependency Injection
- Passed dependencies (e.g., `userId`, `previousForm`) via constructors.
- Example: `CustomExerciseForm` is flexible and testable.

### High Cohesion
- Grouped related logic within classes.
- Example: `ProgressChartForm` contains all chart logic.

### Low Coupling
- Forms interact with DB through `IDatabaseService`, not SQLite directly.
- Example: `GetExercises()` usage in `ExerciseSelectionForm`.

### Open/Closed Principle (OCP)
- Classes open for extension, closed for modification.
- Example: Add DB methods without changing forms.

### Liskov Substitution Principle (LSP)
- Forms can substitute their base without breaking logic.
- Example: `OnFormClosing` overrides maintain flow.

### Interface Segregation Principle (ISP)
- `IDatabaseService` includes only needed methods.
- Example: `ProgressChartForm` uses only `GetExercises`, `GetChartData`.

### Dependency Inversion Principle (DIP)
- Forms depend on abstractions, not concrete implementations.
- Example: Uses `db.AddCustomExercise`, unaware of SQLite.

### Error Handling
- Used try-catch blocks to keep objects valid.
- Example: `AddCustomExercise` catches `SQLiteException` for duplicates.

---

## How to Use

### Open the Project
1. Open `Weight Tracker.sln` in Visual Studio 2022.
2. Ensure `weight_tracker.db` is in `bin\Debug\net8.0-windows`.

### Run the Application
- Press `F5` to start in Debug mode.
- Splash screen → Login Form.

### Test the Workflow

#### Login/Register
- Register a user (e.g., username: `test`, password: `pass`), then log in.

#### Main Menu
- Navigate to "Select Exercise".

#### Exercise Selection
- View default exercises (e.g., “Bench Press”).
- Click "Add Custom", enter "Custom Press", select "Dumbbell", and save.
- Confirm "Custom Press" appears in the list.

#### Workout Logging
- Select "Custom Press", log a workout (e.g., 25 kg, 3 sets, 2 reps).

#### Progress Chart
- Go to "Progress Chart", select "Custom Press" and unit `kg`.

#### History
- View logged workouts in "Workout History".
