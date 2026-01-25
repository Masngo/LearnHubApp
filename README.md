# LearnHubApp

This repository contains a Flutter-based learning platform called **LearnHub**.

Below is the main application code (for easy viewing):

A new Flutter project created with FlutLab - https://flutlab.io/editor/dd2ba917-f0da-4846-b9be-55684ff322cd 
## Getting Started

A few resources to get you started if this is your first Flutter project:

- https://flutter.dev/docs/get-started/codelab
- https://flutter.dev/docs/cookbook

For help getting started with Flutter, view our
https://flutter.dev/docs, which offers tutorials,
samples, guidance on mobile development, and a full API reference.

## Getting Started: FlutLab - Flutter Online IDE

- How to use FlutLab? Please, view our https://flutlab.io/docs
- Join the discussion and conversation on https://flutlab.io/residents

# LearnHub – Flutter Learning Platform

This project is a Flutter-based learning platform built from a Figma UI design.  
It demonstrates dynamic UI, state handling, form validation, and simulated backend integration.

## Features

- Authentication flow (Login, Sign Up, Forgot Password)
- Form validation and user input handling
- Simulated backend using Future.delayed for API calls
- Loading indicators and error/success feedback
- Dashboard with progress indicator
- Program listing connected to mock API data
- Search functionality for programs
- Program details with expandable curriculum
- Feedback form with validation and submission handling
- Bottom navigation (Dashboard, Programs, Profile)
- Logout functionality

## Demo Login

Username: admin  
Password: admin  

## Technologies

- Flutter
- Material UI
- State management using StatefulWidget

This project was built as part of the SOA Package – Handling State & Loading Indicators assignment.

## LearnHuApp Screenshots

![LearHubApp4](https://github.com/user-attachments/assets/c751e108-9cda-45b6-9807-8067298f044f) 


![LearnHubApp10](https://github.com/user-attachments/assets/54f51d6f-9bfa-4d04-930a-8f47527931b4)


![LearnHubApp11](https://github.com/user-attachments/assets/18f5f2a8-964f-499c-bb6b-6e5302251409)


![LearHubApp4](https://github.com/user-attachments/assets/1ee8ff9e-39bd-4121-be0c-882fcd6b5163)


![LearnHubApp2](https://github.com/user-attachments/assets/e5cc377a-e369-4389-afb1-81de3e68ad7f)   


![LearnHubApp3](https://github.com/user-attachments/assets/c9eb6e22-9e64-4619-a256-425e87be20d8)   









---

## 📌 Main Application Code (main.dart)

```dart

// FULL CODE HERE

import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(const LearnHubApp());
}

class LearnHubApp extends StatelessWidget {
  const LearnHubApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'LearnHub',
      theme: ThemeData(
        primaryColor: const Color(0xFF6C63FF),
        scaffoldBackgroundColor: const Color(0xFFF2F5FF),
      ),
      home: const LoginScreen(),
    );
  }
}

/* ================= AUTH SCREENS ================= */

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final _formKey = GlobalKey<FormState>();
  bool loading = false;
  String user = "", pass = "";

  Future<void> fakeLogin() async {
    setState(() => loading = true);
    await Future.delayed(const Duration(seconds: 2));

    if (user == "admin" && pass == "admin") {
      Navigator.pushReplacement(
          context, MaterialPageRoute(builder: (_) => const MainScreen()));
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text("Invalid login details")));
    }

    setState(() => loading = false);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(24),
          child: Column(mainAxisAlignment: MainAxisAlignment.center, children: [
            const Icon(Icons.school, size: 70, color: Color(0xFF6C63FF)),
            const SizedBox(height: 10),
            const Text("LearnHub",
                style: TextStyle(fontSize: 28, fontWeight: FontWeight.bold)),
            const SizedBox(height: 30),

            Form(
              key: _formKey,
              child: Column(children: [
                TextFormField(
                  decoration: const InputDecoration(hintText: "Username"),
                  onSaved: (v) => user = v!,
                  validator: (v) => v!.isEmpty ? "Enter username" : null,
                ),
                const SizedBox(height: 15),
                TextFormField(
                  obscureText: true,
                  decoration: const InputDecoration(hintText: "Password"),
                  onSaved: (v) => pass = v!,
                  validator: (v) => v!.isEmpty ? "Enter password" : null,
                ),
                const SizedBox(height: 25),

                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: loading
                        ? null
                        : () {
                            if (_formKey.currentState!.validate()) {
                              _formKey.currentState!.save();
                              fakeLogin();
                            }
                          },
                    child: loading
                        ? const CircularProgressIndicator(color: Colors.white)
                        : const Text("Sign In"),
                  ),
                ),

                TextButton(
                  onPressed: () => Navigator.push(context,
                      MaterialPageRoute(builder: (_) => const SignUpScreen())),
                  child: const Text("Create Account"),
                ),
                TextButton(
                  onPressed: () => Navigator.push(
                      context,
                      MaterialPageRoute(
                          builder: (_) => const ForgotPasswordScreen())),
                  child: const Text("Forgot Password?"),
                ),
              ]),
            )
          ]),
        ),
      ),
    );
  }
}

/* ---------------- SIGN UP ---------------- */

class SignUpScreen extends StatefulWidget {
  const SignUpScreen({super.key});
  @override
  State<SignUpScreen> createState() => _SignUpScreenState();
}

class _SignUpScreenState extends State<SignUpScreen> {
  bool loading = false;

  Future<void> fakeRegister() async {
    setState(() => loading = true);
    await Future.delayed(const Duration(seconds: 2));
    setState(() => loading = false);

    ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text("Account created successfully")));
    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Create Account")),
      body: Padding(
        padding: const EdgeInsets.all(24),
        child: Column(children: [
          TextField(decoration: const InputDecoration(hintText: "Full Name")),
          const SizedBox(height: 15),
          TextField(decoration: const InputDecoration(hintText: "Email")),
          const SizedBox(height: 15),
          TextField(
              obscureText: true,
              decoration: const InputDecoration(hintText: "Password")),
          const SizedBox(height: 30),

          ElevatedButton(
            onPressed: loading ? null : fakeRegister,
            child: loading
                ? const CircularProgressIndicator(color: Colors.white)
                : const Text("Register"),
          ),
        ]),
      ),
    );
  }
}

/* ---------------- FORGOT PASSWORD ---------------- */

class ForgotPasswordScreen extends StatefulWidget {
  const ForgotPasswordScreen({super.key});
  @override
  State<ForgotPasswordScreen> createState() => _ForgotPasswordScreenState();
}

class _ForgotPasswordScreenState extends State<ForgotPasswordScreen> {
  bool loading = false;

  Future<void> fakeReset() async {
    setState(() => loading = true);
    await Future.delayed(const Duration(seconds: 2));
    setState(() => loading = false);

    ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text("Reset link sent to email")));
    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Forgot Password")),
      body: Padding(
        padding: const EdgeInsets.all(24),
        child: Column(children: [
          TextField(decoration: const InputDecoration(hintText: "Enter Email")),
          const SizedBox(height: 30),

          ElevatedButton(
            onPressed: loading ? null : fakeReset,
            child: loading
                ? const CircularProgressIndicator(color: Colors.white)
                : const Text("Send Reset Link"),
          ),
        ]),
      ),
    );
  }
}

/* ================= MAIN APP ================= */

class MainScreen extends StatefulWidget {
  const MainScreen({super.key});
  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int index = 0;

  final screens = const [
    DashboardScreen(),
    ProgramListScreen(),
    ProfileScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: screens[index],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: index,
        selectedItemColor: const Color(0xFF6C63FF),
        onTap: (i) => setState(() => index = i),
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
          BottomNavigationBarItem(icon: Icon(Icons.book), label: "Programs"),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: "Profile"),
        ],
      ),
    );
  }
}

/* ================= DASHBOARD ================= */

class DashboardScreen extends StatelessWidget {
  const DashboardScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Dashboard")),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
          const Text("Welcome back!",
              style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold)),
          const SizedBox(height: 20),

          const Text("Continue Learning"),
          const SizedBox(height: 8),
          LinearProgressIndicator(
            value: 0.4,
            color: const Color(0xFF6C63FF),
            backgroundColor: Colors.grey.shade300,
          ),
          const SizedBox(height: 30),

          stat("3", "Courses"),
          stat("42", "Hours"),
          stat("2", "Certificates"),
        ]),
      ),
    );
  }

  Widget stat(String v, String l) => Card(
        child: ListTile(
          leading: const Icon(Icons.star, color: Color(0xFF6C63FF)),
          title: Text(v, style: const TextStyle(fontSize: 22)),
          subtitle: Text(l),
        ),
      );
}

/* ================= PROGRAM LIST WITH SEARCH + FAKE API ================= */

class ProgramListScreen extends StatefulWidget {
  const ProgramListScreen({super.key});

  @override
  State<ProgramListScreen> createState() => _ProgramListScreenState();
}

class _ProgramListScreenState extends State<ProgramListScreen> {
  bool loading = true;
  List allPrograms = [];
  List filteredPrograms = [];

  @override
  void initState() {
    super.initState();
    fakeFetchPrograms();
  }

  Future<void> fakeFetchPrograms() async {
    await Future.delayed(const Duration(seconds: 2));

    allPrograms = [
      {"title": "Flutter Basics", "duration": "3 weeks"},
      {"title": "HTML Fundamentals", "duration": "2 weeks"},
      {"title": "CSS Styling", "duration": "2 weeks"},
      {"title": "JavaScript", "duration": "4 weeks"},
    ];

    filteredPrograms = allPrograms;
    setState(() => loading = false);
  }

  void search(String text) {
    filteredPrograms = allPrograms
        .where((p) =>
            p["title"].toLowerCase().contains(text.toLowerCase()))
        .toList();
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Programs")),
      body: loading
          ? const Center(child: CircularProgressIndicator())
          : Column(children: [
              Padding(
                padding: const EdgeInsets.all(12),
                child: TextField(
                  decoration: const InputDecoration(
                      hintText: "Search program...",
                      prefixIcon: Icon(Icons.search)),
                  onChanged: search,
                ),
              ),
              Expanded(
                child: ListView.builder(
                    itemCount: filteredPrograms.length,
                    itemBuilder: (context, i) {
                      final p = filteredPrograms[i];
                      return Card(
                        child: ListTile(
                          title: Text(p["title"]),
                          subtitle: Text("Duration: ${p["duration"]}"),
                          onTap: () {
                            Navigator.push(
                                context,
                                MaterialPageRoute(
                                    builder: (_) => ProgramDetailsScreen(
                                        title: p["title"])));
                          },
                        ),
                      );
                    }),
              )
            ]),
    );
  }
}

/* ================= PROGRAM DETAILS + FEEDBACK ================= */

class ProgramDetailsScreen extends StatelessWidget {
  final String title;
  const ProgramDetailsScreen({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Course Details")),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(children: [
          Text(title,
              style:
                  const TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
          const SizedBox(height: 20),

          const ExpansionTile(title: Text("Introduction")),
          const ExpansionTile(title: Text("UI Design")),
          const ExpansionTile(title: Text("State Management")),
          const ExpansionTile(title: Text("Final Project")),

          const SizedBox(height: 30),

          ElevatedButton(
            onPressed: () {
              Navigator.push(context,
                  MaterialPageRoute(builder: (_) => const FeedbackScreen()));
            },
            child: const Text("Give Feedback"),
          )
        ]),
      ),
    );
  }
}

/* ================= FEEDBACK FORM ================= */

class FeedbackScreen extends StatefulWidget {
  const FeedbackScreen({super.key});

  @override
  State<FeedbackScreen> createState() => _FeedbackScreenState();
}

class _FeedbackScreenState extends State<FeedbackScreen> {
  bool loading = false;

  Future<void> submitFeedback() async {
    setState(() => loading = true);
    await Future.delayed(const Duration(seconds: 2));
    setState(() => loading = false);

    ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text("Feedback submitted successfully")));
    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Feedback")),
      body: Padding(
        padding: const EdgeInsets.all(24),
        child: Column(children: [
          TextField(decoration: const InputDecoration(hintText: "Your Name")),
          const SizedBox(height: 15),
          TextField(decoration: const InputDecoration(hintText: "Your Email")),
          const SizedBox(height: 15),
          TextField(
            maxLines: 4,
            decoration: const InputDecoration(hintText: "Your Feedback"),
          ),
          const SizedBox(height: 30),

          ElevatedButton(
            onPressed: loading ? null : submitFeedback,
            child: loading
                ? const CircularProgressIndicator(color: Colors.white)
                : const Text("Submit Feedback"),
          ),
        ]),
      ),
    );
  }
}

/* ================= PROFILE ================= */

class ProfileScreen extends StatelessWidget {
  const ProfileScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Profile")),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(children: [
          const CircleAvatar(
            radius: 40,
            backgroundColor: Color(0xFF6C63FF),
            child: Text("AD",
                style: TextStyle(color: Colors.white, fontSize: 24)),
          ),
          const SizedBox(height: 20),
          const Text("Admin User",
              style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold)),
          const SizedBox(height: 40),

          ElevatedButton(
            style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
            onPressed: () {
              Navigator.pushAndRemoveUntil(
                  context,
                  MaterialPageRoute(builder: (_) => const LoginScreen()),
                  (route) => false);
            },
            child: const Text("Logout"),
          )
        ]),
      ),
    );
  }
}
