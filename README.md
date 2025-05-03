Weight Tracker System
The Weight Tracker System is a Windows Forms application I developed using C# and .NET 8.0 to help users track workouts, manage exercises, and visualize progress. I implemented user authentication, workout logging, progress charts, and custom exercise creation, using SQLite for data storage. My work ensures robust functionality, such as fixing the issue where custom exercises didn’t appear in the exercise list, and applies object-oriented programming (OOP) principles for a modular design.
Prerequisites

Operating System: Windows 10 or later.
.NET 8.0 Runtime: Required to run the application.
Visual Studio 2022: For opening and running the project.
Database: weight_tracker.db (included in the project folder).
Dependencies: System.Data.SQLite.Core v1.0.118.0 (included in the project).

My Contributions
I designed and implemented the core functionality of the Weight Tracker System, focusing on the following components:
DatabaseHelper.cs

What I Did: I created a database management class to handle all SQLite operations. I defined the IDatabaseService interface and implemented methods for user authentication, exercise retrieval, workout logging, and custom exercise creation. I added support for equipment_type and plate_weights in the exercises and workouts tables, ensuring data integrity with foreign keys. I included detailed debug logging to troubleshoot issues.
Key Features:
Initialize users, exercises, workouts tables with schema.
Methods: AuthenticateUser, RegisterUser, GetExercises, AddCustomExercise, LogWorkout, GetChartData.
Fixed database errors by handling SQLite constraints (e.g., duplicate exercise names).



CustomExerciseForm.cs

What I Did: I developed a form to add custom exercises with name and equipment type (Barbell/Dumbbell). I implemented input validation to ensure names are valid (letters, numbers, spaces, max 50 characters) and equipment is selected. I fixed the issue where custom exercises weren’t saved correctly by enhancing error handling in saveButton_Click.
Key Features:
Validates input using Regex and checks for duplicates.
Saves exercises via AddCustomExercise, showing success/error messages.
Navigates back to ExerciseSelectionForm.



ExerciseSelectionForm.cs

What I Did: I created a form to select exercises or add custom ones. I fixed the critical bug where custom exercises didn’t appear in the exercise list by adding RefreshExerciseList in OnShown, ensuring the ComboBox updates after adding a custom exercise. I implemented navigation to WorkoutInputForm and CustomExerciseForm.
Key Features:
Populates ComboBox with GetExercises, refreshed on form show.
Handles selection, custom exercise addition, and back navigation.
Displays errors if no exercises are available.



ProgressChartForm.cs

What I Did: I built a form to visualize workout progress as a line chart. I implemented dynamic chart rendering using PictureBox, fixed issues with incorrect column references (weight instead of total_weight), and prevented date label overlap by showing only up to 5 labels. I ensured the chart supports custom exercises and kg/lbs units.
Key Features:
Fetches data with GetChartData, plots weight vs. date.
Handles empty/invalid data with clear error messages.
Updates chart on exercise/unit change.



Other Forms

LoginForm.cs: I implemented user login and registration with secure password hashing.
WorkoutInputForm.cs: I created workout logging with support for equipment type and plate weights.
MainMenuForm.cs, SplashForm.cs, WorkoutHistoryForm.cs, RegistrationForm.cs: I developed these for navigation and additional features.

OOP Concepts
I applied a wide range of OOP principles to ensure the Weight Tracker System is modular, maintainable, and scalable:

Encapsulation:

I protected internal state by using private fields (e.g., DatabaseHelper.connectionString, CustomExerciseForm.db, userId). Access is controlled via public methods or constructors, preventing external interference.
Example: In ProgressChartForm, chartData is private, updated only by UpdateChart.


Inheritance:

I leveraged inheritance by deriving all forms from System.Windows.Forms.Form, inheriting properties like Show and Hide. This allowed me to reuse base class functionality while customizing behavior.
Example: ExerciseSelectionForm inherits OnShown and overrides it to refresh the exercise list.


Polymorphism:

I used method overriding to customize form behavior. For instance, I overrode OnFormClosing in all forms to ensure proper navigation (e.g., showing previousForm).
Example: In ProgressChartForm, I implemented ChartPictureBox_Paint to provide custom chart rendering, a form of runtime polymorphism.


Abstraction:

I defined the IDatabaseService interface to abstract database operations, hiding SQLite implementation details. This allows swapping databases (e.g., to MySQL) without changing form logic.
Example: DatabaseHelper implements IDatabaseService, exposing only methods like AddCustomExercise.


Single Responsibility Principle (SRP):

I ensured each class/method has one responsibility. For example, CustomExerciseForm only handles custom exercise creation, while DatabaseHelper.AddCustomExercise only inserts exercises into the database.
Example: ExerciseSelectionForm.RefreshExerciseList solely updates the ComboBox.


Dependency Injection:

I passed dependencies (e.g., userId, previousForm) via constructors, reducing coupling. This makes forms reusable and testable.
Example: CustomExerciseForm receives userId and previousForm, enabling flexible navigation.


High Cohesion:

I grouped related functionality within classes. For instance, ProgressChartForm contains all chart-related logic (data fetching, rendering, UI updates), ensuring cohesive behavior.
Example: UpdateChart and ChartPictureBox_Paint work together to render charts.


Low Coupling:

I minimized dependencies between classes. Forms interact with DatabaseHelper via IDatabaseService, not directly with SQLite, reducing tight coupling.
Example: ExerciseSelectionForm uses db.GetExercises() without knowing database details.


Open/Closed Principle (OCP):

I designed classes to be open for extension but closed for modification. For example, new database operations can be added to IDatabaseService without changing existing form code.
Example: Adding a new method to DatabaseHelper doesn’t affect CustomExerciseForm.


Liskov Substitution Principle (LSP):

I ensured derived classes (e.g., ProgressChartForm) can substitute Form without breaking functionality. All forms adhere to the base class’s contract.
Example: OnFormClosing overrides maintain navigation consistency.


Interface Segregation Principle (ISP):

I kept IDatabaseService focused, including only methods needed by forms (e.g., GetExercises, AddCustomExercise). This prevents forms from depending on unused methods.
Example: ProgressChartForm only uses GetExercises and GetChartData.


Dependency Inversion Principle (DIP):

I made high-level modules (forms) depend on abstractions (IDatabaseService) rather than concrete implementations (DatabaseHelper), enhancing flexibility.
Example: CustomExerciseForm uses db.AddCustomExercise, unaware of SQLite.


Error Handling as an OOP Practice:

I implemented robust error handling using try-catch blocks, treating exceptions as part of the object’s behavior. This ensures objects remain in a valid state.
Example: DatabaseHelper.AddCustomExercise catches SQLiteException for duplicates, returning false.



How to Use

Open the Project:
Open Weight Tracker.sln in Visual Studio 2022.
Ensure weight_tracker.db is in the project’s bin\Debug\net8.0-windows folder.


Run the Application:
Press F5 to start in Debug mode.
The splash screen appears, followed by the login form.


Test the Workflow:
Login/Register: Register a user (e.g., username: “test”, password: “pass”), then log in.
Main Menu: Navigate to “Select Exercise”.
Exercise Selection:
View default exercises (e.g., “Bench Press”).
Click “Add Custom”, enter “Custom Press”, select “Dumbbell”, and save.
Verify “Custom Press” appears in the exercise list.


Workout Logging: Select “Custom Press”, log a workout (e.g., 25 kg, 3 sets, 2 reps).
Progress Chart: Go to “Progress Chart”, select “Custom Press” and “kg” to view the chart.
History: Check “Workout History” for logged workouts.



Chart Issues:
Check: Ensure workouts exist for the selected exercise/unit.
Fix: Log a workout in WorkoutInputForm first.

