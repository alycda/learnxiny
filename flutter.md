---
category: framework
name: Flutter
filename: learnflutter.dart
contributors:
  - ["Learn X in Y Minutes", "https://learnxinyminutes.com/"]
---

Flutter is Google's UI toolkit for building natively compiled applications
for mobile, web, and desktop from a single codebase. It uses Dart as its
programming language and provides a rich set of pre-built widgets.

This tutorial assumes familiarity with Dart. Check out the Dart tutorial at
[https://learnxinyminutes.com/dart/](https://learnxinyminutes.com/dart/)

```dart
import 'package:flutter/material.dart';

// Every Flutter app starts with a main() function that calls runApp()
// runApp() takes a Widget and makes it the root of the widget tree
void main() {
  runApp(const MyApp());
}

/////////////////////////////////////////////////////////////////////////////
// 1. WIDGETS BASICS
/////////////////////////////////////////////////////////////////////////////

// Everything in Flutter is a widget. Widgets describe what their view
// should look like given their current configuration and state.

// There are two types of widgets: StatelessWidget and StatefulWidget

// StatelessWidget - immutable, all values are final
// Use when the UI doesn't need to change dynamically
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // Every widget must override the build() method
  // BuildContext contains information about the widget's location in the tree
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Learn Flutter',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const HomePage(),
    );
  }
}

// StatefulWidget - can change over time
// Use when the UI needs to update dynamically
class HomePage extends StatefulWidget {
  const HomePage({super.key});

  // Creates the mutable state for this widget
  @override
  State<HomePage> createState() => _HomePageState();
}

// State class contains the mutable state and build logic
// Prefix with underscore to make it private
class _HomePageState extends State<HomePage> {
  int _counter = 0;

  void _incrementCounter() {
    // setState() notifies Flutter that state has changed
    // This triggers a rebuild of the widget
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home')),
      body: Center(
        child: Text('Counter: $_counter'),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        child: const Icon(Icons.add),
      ),
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 2. COMMON WIDGETS
/////////////////////////////////////////////////////////////////////////////

class CommonWidgetsExample extends StatelessWidget {
  const CommonWidgetsExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Text widget displays a string of text with single style
        const Text(
          'Hello, Flutter!',
          style: TextStyle(
            fontSize: 24,
            fontWeight: FontWeight.bold,
            color: Colors.blue,
          ),
        ),

        // Container - a convenience widget for decoration and sizing
        Container(
          width: 200,
          height: 100,
          margin: const EdgeInsets.all(10),
          padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 10),
          decoration: BoxDecoration(
            color: Colors.amber,
            borderRadius: BorderRadius.circular(10),
            boxShadow: const [
              BoxShadow(color: Colors.grey, blurRadius: 5),
            ],
          ),
          child: const Text('Styled Container'),
        ),

        // Image widget displays images from various sources
        Image.network('https://flutter.dev/images/flutter-logo-sharing.png'),
        Image.asset('assets/local_image.png'), // From assets folder

        // Icon widget displays Material icons
        const Icon(Icons.favorite, color: Colors.red, size: 48),

        // Buttons come in various styles
        ElevatedButton(
          onPressed: () => print('Pressed!'),
          child: const Text('Elevated Button'),
        ),
        TextButton(
          onPressed: () {},
          child: const Text('Text Button'),
        ),
        OutlinedButton(
          onPressed: () {},
          child: const Text('Outlined Button'),
        ),
        IconButton(
          icon: const Icon(Icons.thumb_up),
          onPressed: () {},
        ),
      ],
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 3. LAYOUT WIDGETS
/////////////////////////////////////////////////////////////////////////////

class LayoutExample extends StatelessWidget {
  const LayoutExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Row - arranges children horizontally
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly, // Horizontal axis
          crossAxisAlignment: CrossAxisAlignment.center,    // Vertical axis
          children: const [
            Text('Item 1'),
            Text('Item 2'),
            Text('Item 3'),
          ],
        ),

        // Column - arranges children vertically
        Column(
          mainAxisAlignment: MainAxisAlignment.center,    // Vertical axis
          crossAxisAlignment: CrossAxisAlignment.start,   // Horizontal axis
          children: const [
            Text('Row A'),
            Text('Row B'),
            Text('Row C'),
          ],
        ),

        // Stack - overlays children on top of each other
        Stack(
          alignment: Alignment.center,
          children: [
            Container(width: 200, height: 200, color: Colors.red),
            Container(width: 150, height: 150, color: Colors.green),
            Container(width: 100, height: 100, color: Colors.blue),
          ],
        ),

        // Expanded - fills available space in Row/Column
        Row(
          children: [
            Expanded(
              flex: 2, // Takes 2/3 of available space
              child: Container(color: Colors.red, height: 50),
            ),
            Expanded(
              flex: 1, // Takes 1/3 of available space
              child: Container(color: Colors.blue, height: 50),
            ),
          ],
        ),

        // Wrap - wraps children to next line when space runs out
        const Wrap(
          spacing: 8,    // Horizontal gap between children
          runSpacing: 4, // Vertical gap between lines
          children: [
            Chip(label: Text('Tag 1')),
            Chip(label: Text('Tag 2')),
            Chip(label: Text('Tag 3')),
            Chip(label: Text('Tag 4')),
          ],
        ),
      ],
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 4. SCROLLING WIDGETS
/////////////////////////////////////////////////////////////////////////////

class ScrollingExample extends StatelessWidget {
  const ScrollingExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // ListView - scrollable list of widgets
        // Use ListView.builder for long/infinite lists (lazy loading)
        Expanded(
          child: ListView.builder(
            itemCount: 100,
            itemBuilder: (context, index) {
              return ListTile(
                leading: const Icon(Icons.person),
                title: Text('Item $index'),
                subtitle: Text('Subtitle for item $index'),
                trailing: const Icon(Icons.arrow_forward),
                onTap: () => print('Tapped item $index'),
              );
            },
          ),
        ),

        // GridView - scrollable grid of widgets
        Expanded(
          child: GridView.builder(
            gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,       // Number of columns
              crossAxisSpacing: 10,    // Horizontal gap
              mainAxisSpacing: 10,     // Vertical gap
              childAspectRatio: 1.5,   // Width / Height ratio
            ),
            itemCount: 20,
            itemBuilder: (context, index) {
              return Card(
                child: Center(child: Text('Grid $index')),
              );
            },
          ),
        ),

        // SingleChildScrollView - makes a single child scrollable
        const SingleChildScrollView(
          scrollDirection: Axis.horizontal,
          child: Row(
            children: [
              SizedBox(width: 200, child: Card(child: Text('Card 1'))),
              SizedBox(width: 200, child: Card(child: Text('Card 2'))),
              SizedBox(width: 200, child: Card(child: Text('Card 3'))),
            ],
          ),
        ),
      ],
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 5. NAVIGATION
/////////////////////////////////////////////////////////////////////////////

class NavigationExample extends StatelessWidget {
  const NavigationExample({super.key});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        // Push a new route onto the navigation stack
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => const SecondPage(),
          ),
        );
      },
      child: const Text('Go to Second Page'),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Second Page')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                // Pop the current route off the stack
                Navigator.pop(context);
              },
              child: const Text('Go Back'),
            ),
            ElevatedButton(
              onPressed: () {
                // Pop and return data to previous screen
                Navigator.pop(context, 'Returned data');
              },
              child: const Text('Go Back with Data'),
            ),
          ],
        ),
      ),
    );
  }
}

// Named routes - define routes in MaterialApp
class AppWithNamedRoutes extends StatelessWidget {
  const AppWithNamedRoutes({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: '/',
      routes: {
        '/': (context) => const HomePage(),
        '/second': (context) => const SecondPage(),
        '/settings': (context) => const SettingsPage(),
      },
    );
  }
}

class SettingsPage extends StatelessWidget {
  const SettingsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Settings')),
      body: ElevatedButton(
        // Navigate using named route
        onPressed: () => Navigator.pushNamed(context, '/second'),
        child: const Text('Go to Second'),
      ),
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 6. FORMS AND INPUT
/////////////////////////////////////////////////////////////////////////////

class FormExample extends StatefulWidget {
  const FormExample({super.key});

  @override
  State<FormExample> createState() => _FormExampleState();
}

class _FormExampleState extends State<FormExample> {
  // GlobalKey uniquely identifies the form and allows validation
  final _formKey = GlobalKey<FormState>();

  // Controllers to retrieve text field values
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();

  bool _agreedToTerms = false;
  String _selectedOption = 'Option 1';

  @override
  void dispose() {
    // Always dispose controllers to free resources
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  void _submitForm() {
    // validate() runs all TextFormField validators
    if (_formKey.currentState!.validate()) {
      print('Name: ${_nameController.text}');
      print('Email: ${_emailController.text}');
      print('Agreed: $_agreedToTerms');
      print('Option: $_selectedOption');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        children: [
          // TextFormField with validation
          TextFormField(
            controller: _nameController,
            decoration: const InputDecoration(
              labelText: 'Name',
              hintText: 'Enter your name',
              prefixIcon: Icon(Icons.person),
            ),
            validator: (value) {
              if (value == null || value.isEmpty) {
                return 'Please enter your name'; // Error message
              }
              return null; // Valid
            },
          ),

          TextFormField(
            controller: _emailController,
            decoration: const InputDecoration(labelText: 'Email'),
            keyboardType: TextInputType.emailAddress,
            validator: (value) {
              if (value == null || !value.contains('@')) {
                return 'Please enter a valid email';
              }
              return null;
            },
          ),

          // Checkbox
          CheckboxListTile(
            title: const Text('I agree to the terms'),
            value: _agreedToTerms,
            onChanged: (value) {
              setState(() {
                _agreedToTerms = value ?? false;
              });
            },
          ),

          // Radio buttons
          RadioListTile<String>(
            title: const Text('Option 1'),
            value: 'Option 1',
            groupValue: _selectedOption,
            onChanged: (value) {
              setState(() => _selectedOption = value!);
            },
          ),
          RadioListTile<String>(
            title: const Text('Option 2'),
            value: 'Option 2',
            groupValue: _selectedOption,
            onChanged: (value) {
              setState(() => _selectedOption = value!);
            },
          ),

          // Switch
          SwitchListTile(
            title: const Text('Enable notifications'),
            value: _agreedToTerms,
            onChanged: (value) {
              setState(() => _agreedToTerms = value);
            },
          ),

          // Dropdown
          DropdownButton<String>(
            value: _selectedOption,
            items: const [
              DropdownMenuItem(value: 'Option 1', child: Text('Option 1')),
              DropdownMenuItem(value: 'Option 2', child: Text('Option 2')),
              DropdownMenuItem(value: 'Option 3', child: Text('Option 3')),
            ],
            onChanged: (value) {
              setState(() => _selectedOption = value!);
            },
          ),

          ElevatedButton(
            onPressed: _submitForm,
            child: const Text('Submit'),
          ),
        ],
      ),
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 7. ASYNC OPERATIONS
/////////////////////////////////////////////////////////////////////////////

class AsyncExample extends StatefulWidget {
  const AsyncExample({super.key});

  @override
  State<AsyncExample> createState() => _AsyncExampleState();
}

class _AsyncExampleState extends State<AsyncExample> {
  String _data = 'No data';
  bool _isLoading = false;

  // Async function to simulate fetching data
  Future<void> _fetchData() async {
    setState(() => _isLoading = true);

    // Simulating network delay
    await Future.delayed(const Duration(seconds: 2));

    setState(() {
      _data = 'Data loaded at ${DateTime.now()}';
      _isLoading = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Show loading indicator or data
        if (_isLoading)
          const CircularProgressIndicator()
        else
          Text(_data),

        ElevatedButton(
          onPressed: _fetchData,
          child: const Text('Fetch Data'),
        ),
      ],
    );
  }
}

// FutureBuilder - builds widget based on Future state
class FutureBuilderExample extends StatelessWidget {
  const FutureBuilderExample({super.key});

  Future<String> _fetchData() async {
    await Future.delayed(const Duration(seconds: 2));
    return 'Fetched data!';
  }

  @override
  Widget build(BuildContext context) {
    return FutureBuilder<String>(
      future: _fetchData(),
      builder: (context, snapshot) {
        // Check connection state
        if (snapshot.connectionState == ConnectionState.waiting) {
          return const CircularProgressIndicator();
        }

        // Check for errors
        if (snapshot.hasError) {
          return Text('Error: ${snapshot.error}');
        }

        // Data is ready
        if (snapshot.hasData) {
          return Text(snapshot.data!);
        }

        return const Text('No data');
      },
    );
  }
}

// StreamBuilder - builds widget based on Stream events
class StreamBuilderExample extends StatelessWidget {
  const StreamBuilderExample({super.key});

  // Create a stream that emits values
  Stream<int> _counterStream() async* {
    for (int i = 1; i <= 10; i++) {
      await Future.delayed(const Duration(seconds: 1));
      yield i;
    }
  }

  @override
  Widget build(BuildContext context) {
    return StreamBuilder<int>(
      stream: _counterStream(),
      builder: (context, snapshot) {
        if (snapshot.hasData) {
          return Text('Count: ${snapshot.data}');
        }
        return const Text('Waiting...');
      },
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 8. THEMING AND STYLING
/////////////////////////////////////////////////////////////////////////////

class ThemedApp extends StatelessWidget {
  const ThemedApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      // Define app-wide theme
      theme: ThemeData(
        // Color scheme
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,

        // AppBar theme
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.deepPurple,
          foregroundColor: Colors.white,
          elevation: 4,
        ),

        // Text theme
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
          ),
          bodyMedium: TextStyle(
            fontSize: 16,
            color: Colors.grey,
          ),
        ),

        // Button theme
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            padding: const EdgeInsets.symmetric(horizontal: 32, vertical: 16),
          ),
        ),
      ),

      // Dark theme (system will choose based on device settings)
      darkTheme: ThemeData.dark().copyWith(
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.deepPurple,
          brightness: Brightness.dark,
        ),
      ),
      themeMode: ThemeMode.system, // system, light, or dark

      home: const ThemeExamplePage(),
    );
  }
}

class ThemeExamplePage extends StatelessWidget {
  const ThemeExamplePage({super.key});

  @override
  Widget build(BuildContext context) {
    // Access theme values using Theme.of(context)
    final theme = Theme.of(context);

    return Scaffold(
      appBar: AppBar(title: const Text('Themed App')),
      body: Column(
        children: [
          Text(
            'Headline',
            style: theme.textTheme.headlineLarge,
          ),
          Text(
            'Body text',
            style: theme.textTheme.bodyMedium,
          ),
          Container(
            color: theme.colorScheme.primary,
            padding: const EdgeInsets.all(16),
            child: Text(
              'Primary Color',
              style: TextStyle(color: theme.colorScheme.onPrimary),
            ),
          ),
        ],
      ),
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 9. STATE MANAGEMENT WITH INHERITED WIDGET
/////////////////////////////////////////////////////////////////////////////

// Simple state management using InheritedWidget pattern
// For complex apps, consider packages like Provider, Riverpod, or Bloc

class CounterState extends InheritedWidget {
  final int counter;
  final VoidCallback increment;

  const CounterState({
    super.key,
    required this.counter,
    required this.increment,
    required super.child,
  });

  // Helper method to access state from anywhere in the tree
  static CounterState of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<CounterState>()!;
  }

  @override
  bool updateShouldNotify(CounterState oldWidget) {
    return counter != oldWidget.counter;
  }
}

class CounterProvider extends StatefulWidget {
  final Widget child;

  const CounterProvider({super.key, required this.child});

  @override
  State<CounterProvider> createState() => _CounterProviderState();
}

class _CounterProviderState extends State<CounterProvider> {
  int _counter = 0;

  void _increment() {
    setState(() => _counter++);
  }

  @override
  Widget build(BuildContext context) {
    return CounterState(
      counter: _counter,
      increment: _increment,
      child: widget.child,
    );
  }
}

// Usage of inherited state
class CounterDisplay extends StatelessWidget {
  const CounterDisplay({super.key});

  @override
  Widget build(BuildContext context) {
    // Access state without passing it through constructors
    final state = CounterState.of(context);

    return Column(
      children: [
        Text('Counter: ${state.counter}'),
        ElevatedButton(
          onPressed: state.increment,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 10. LIFECYCLE METHODS
/////////////////////////////////////////////////////////////////////////////

class LifecycleExample extends StatefulWidget {
  const LifecycleExample({super.key});

  @override
  State<LifecycleExample> createState() => _LifecycleExampleState();
}

class _LifecycleExampleState extends State<LifecycleExample> {
  // Called when the State object is first created
  @override
  void initState() {
    super.initState();
    print('initState: Widget is being created');
    // Good place to initialize data, start animations, subscribe to streams
  }

  // Called when the widget's dependencies change
  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    print('didChangeDependencies: Dependencies changed');
    // Called after initState and when InheritedWidget changes
  }

  // Called when parent rebuilds with new configuration
  @override
  void didUpdateWidget(LifecycleExample oldWidget) {
    super.didUpdateWidget(oldWidget);
    print('didUpdateWidget: Widget configuration changed');
    // Compare oldWidget with widget to respond to changes
  }

  // Called when the State object is permanently removed
  @override
  void dispose() {
    print('dispose: Widget is being destroyed');
    // Clean up: cancel timers, close streams, dispose controllers
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    print('build: Widget is building/rebuilding');
    return const Text('Lifecycle Example');
  }
}

/////////////////////////////////////////////////////////////////////////////
// 11. ANIMATIONS
/////////////////////////////////////////////////////////////////////////////

class AnimationExample extends StatefulWidget {
  const AnimationExample({super.key});

  @override
  State<AnimationExample> createState() => _AnimationExampleState();
}

// SingleTickerProviderStateMixin provides a Ticker for animations
class _AnimationExampleState extends State<AnimationExample>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();

    // Controller manages the animation
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this, // Prevents off-screen animations
    );

    // Tween defines the range of values
    _animation = Tween<double>(begin: 0, end: 300).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeInOut),
    );

    // Start the animation
    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Container(
          width: _animation.value,
          height: _animation.value,
          color: Colors.blue,
        );
      },
    );
  }
}

// Implicit animations - simple animated widgets
class ImplicitAnimationExample extends StatefulWidget {
  const ImplicitAnimationExample({super.key});

  @override
  State<ImplicitAnimationExample> createState() =>
      _ImplicitAnimationExampleState();
}

class _ImplicitAnimationExampleState extends State<ImplicitAnimationExample> {
  bool _expanded = false;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // AnimatedContainer automatically animates property changes
        AnimatedContainer(
          duration: const Duration(milliseconds: 500),
          curve: Curves.easeInOut,
          width: _expanded ? 200 : 100,
          height: _expanded ? 200 : 100,
          color: _expanded ? Colors.blue : Colors.red,
          child: const Center(child: Text('Tap button')),
        ),

        // AnimatedOpacity fades widget in/out
        AnimatedOpacity(
          duration: const Duration(milliseconds: 500),
          opacity: _expanded ? 1.0 : 0.5,
          child: const Text('Fading text'),
        ),

        ElevatedButton(
          onPressed: () => setState(() => _expanded = !_expanded),
          child: const Text('Toggle'),
        ),
      ],
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 12. DIALOGS AND BOTTOM SHEETS
/////////////////////////////////////////////////////////////////////////////

class DialogExample extends StatelessWidget {
  const DialogExample({super.key});

  void _showAlertDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Alert'),
        content: const Text('This is an alert dialog.'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          TextButton(
            onPressed: () {
              Navigator.pop(context);
              print('Confirmed!');
            },
            child: const Text('OK'),
          ),
        ],
      ),
    );
  }

  void _showBottomSheet(BuildContext context) {
    showModalBottomSheet(
      context: context,
      builder: (context) => Container(
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            const Text('Bottom Sheet'),
            ListTile(
              leading: const Icon(Icons.share),
              title: const Text('Share'),
              onTap: () => Navigator.pop(context),
            ),
            ListTile(
              leading: const Icon(Icons.delete),
              title: const Text('Delete'),
              onTap: () => Navigator.pop(context),
            ),
          ],
        ),
      ),
    );
  }

  void _showSnackBar(BuildContext context) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: const Text('This is a snackbar'),
        action: SnackBarAction(
          label: 'Undo',
          onPressed: () => print('Undo pressed'),
        ),
        duration: const Duration(seconds: 3),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        ElevatedButton(
          onPressed: () => _showAlertDialog(context),
          child: const Text('Show Dialog'),
        ),
        ElevatedButton(
          onPressed: () => _showBottomSheet(context),
          child: const Text('Show Bottom Sheet'),
        ),
        ElevatedButton(
          onPressed: () => _showSnackBar(context),
          child: const Text('Show SnackBar'),
        ),
      ],
    );
  }
}

/////////////////////////////////////////////////////////////////////////////
// 13. KEYS
/////////////////////////////////////////////////////////////////////////////

// Keys help Flutter identify which widgets have changed in a list
class KeysExample extends StatelessWidget {
  const KeysExample({super.key});

  @override
  Widget build(BuildContext context) {
    final items = ['Apple', 'Banana', 'Cherry'];

    return ListView(
      children: items.map((item) {
        // Use ValueKey for unique identification based on data
        return ListTile(
          key: ValueKey(item),
          title: Text(item),
        );
      }).toList(),
    );
  }
}

// Key types:
// - ValueKey: Use when items have unique values
// - ObjectKey: Use when items are objects without unique fields
// - UniqueKey: Creates a unique key (rarely needed)
// - GlobalKey: Access widget state from anywhere (use sparingly)

/////////////////////////////////////////////////////////////////////////////
// 14. CUSTOM WIDGETS
/////////////////////////////////////////////////////////////////////////////

// Create reusable widgets by extracting common patterns

class CustomCard extends StatelessWidget {
  final String title;
  final String subtitle;
  final IconData icon;
  final VoidCallback? onTap;

  const CustomCard({
    super.key,
    required this.title,
    required this.subtitle,
    this.icon = Icons.info,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        leading: Icon(icon),
        title: Text(title),
        subtitle: Text(subtitle),
        trailing: const Icon(Icons.chevron_right),
        onTap: onTap,
      ),
    );
  }
}

// Usage
class CustomWidgetUsage extends StatelessWidget {
  const CustomWidgetUsage({super.key});

  @override
  Widget build(BuildContext context) {
    return const Column(
      children: [
        CustomCard(
          title: 'First Card',
          subtitle: 'This is the first custom card',
          icon: Icons.one_k,
        ),
        CustomCard(
          title: 'Second Card',
          subtitle: 'This is another custom card',
          icon: Icons.two_k,
        ),
      ],
    );
  }
}
```

## Further Reading

- [Flutter Official Documentation](https://docs.flutter.dev/)
- [Flutter Widget Catalog](https://docs.flutter.dev/ui/widgets)
- [Flutter Cookbook](https://docs.flutter.dev/cookbook)
- [DartPad - Online Flutter Editor](https://dartpad.dev/)
- [Pub.dev - Dart/Flutter Packages](https://pub.dev/)
- [Flutter YouTube Channel](https://www.youtube.com/flutterdev)
