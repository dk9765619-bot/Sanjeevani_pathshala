# Sanjeevani_pathshalaimport 'package:flutter/material.dart';

void main() {
  runApp(const SanjeevaniPathsalaApp());
}

class SanjeevaniPathsalaApp extends StatelessWidget {
  const SanjeevaniPathsalaApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sanjeevani Pathsala',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
        scaffoldBackgroundColor: Colors.grey[100],
      ),
      home: const DashboardScreen(),
    );
  }
}

class DashboardScreen extends StatefulWidget {
  const DashboardScreen({super.key});

  @override
  State<DashboardScreen> createState() => _DashboardScreenState();
}

class _DashboardScreenState extends State<DashboardScreen> {
  int _currentIndex = 0;

  final List<Widget> _screens = [
    const HomeScreen(),
    const NoticeScreen(),
    const PaymentScreen(), // Added Payment Screen here
    const ComplaintScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('SANJEEVANI PATHSALA', style: TextStyle(fontWeight: FontWeight.bold, fontSize: 18)),
        backgroundColor: Colors.blue[800],
        foregroundColor: Colors.white,
      ),
      body: _screens[_currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        type: BottomNavigationBarType.fixed, // Fixed type for 4 items
        onTap: (index) {
          setState(() {
            _currentIndex = index;
          });
        },
        selectedItemColor: Colors.blue[800],
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
          BottomNavigationBarItem(icon: Icon(Icons.notifications), label: 'Notices'),
          BottomNavigationBarItem(icon: Icon(Icons.payment), label: 'Fee Pay'), // New Tab
          BottomNavigationBarItem(icon: Icon(Icons.report_problem), label: 'Complain'),
        ],
      ),
    );
  }
}

// 1. HOME SCREEN WITH ALL MODULES (Attendance, Homework, Tests, Logo)
class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          // Banner with Institute Image Logo
          Container(
            height: 180,
            width: double.infinity,
            decoration: BoxDecoration(
              borderRadius: BorderRadius.circular(12),
              image: const DecorationImage(
                image: AssetImage('assets/logo.jpg'), // Uses your uploaded image[span_0](start_span)[span_0](end_span)
                fit: BoxFit.cover,
              ),
            ),
            child: Container(
              decoration: BoxDecoration(
                borderRadius: BorderRadius.circular(12),
                gradient: LinearGradient(
                  begin: Alignment.bottomCenter,
                  end: Alignment.topCenter,
                  colors: [Colors.black.withOpacity(0.7), Colors.transparent],
                ),
              ),
              padding: const EdgeInsets.all(16),
              alignment: Alignment.bottomLeft,
              child: const Text(
                'Sanjeevani Pathsala\nSasaram, Rohtas, Bihar[span_1](start_span)[span_1](end_span)',
                style: TextStyle(color: Colors.white, fontSize: 16, fontWeight: FontWeight.bold),
              ),
            ),
          ),
          const SizedBox(height: 20),
          const Text('Quick Access', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
          const SizedBox(height: 12),
          
          // Grid Menu Options
          GridView.count(
            crossAxisCount: 2,
            shrinkWrap: true,
            physics: const NeverScrollableScrollPhysics(),
            crossAxisSpacing: 12,
            mainAxisSpacing: 12,
            childAspectRatio: 1.3,
            children: [
              DashboardCard(
                title: 'Attendance',
                icon: Icons.calendar_today,
                color: Colors.green,
                onTap: () => Navigator.push(context, MaterialPageRoute(builder: (_) => const AttendanceScreen())),
              ),
              DashboardCard(
                title: 'Homework',
                icon: Icons.book,
                color: Colors.orange,
                onTap: () => Navigator.push(context, MaterialPageRoute(builder: (_) => const HomeworkScreen())),
              ),
              DashboardCard(
                title: 'Weekly Test & Marks',
                icon: Icons.bar_chart,
                color: Colors.blue,
                onTap: () => Navigator.push(context, MaterialPageRoute(builder: (_) => const MarksScreen())),
              ),
              DashboardCard(
                title: 'Fee Payment',
                icon: Icons.payment,
                color: Colors.teal,
                onTap: () => Navigator.push(context, MaterialPageRoute(builder: (_) => const PaymentScreenNav())),
              ),
            ],
          ),
        ],
      ),
    );
  }
}

class DashboardCard extends StatelessWidget {
  const DashboardCard({super.key, required this.title, required this.icon, required this.color, required this.onTap});

  final String title;
  final IconData icon;
  final Color color;
  final VoidCallback onTap;

  @override
  Widget build(BuildContext context) {
    return InkWell(
      onTap: onTap,
      borderRadius: BorderRadius.circular(12),
      child: Container(
        decoration: BoxDecoration(
          color: Colors.white,
          borderRadius: BorderRadius.circular(12),
          boxShadow: [BoxShadow(color: Colors.grey.withOpacity(0.2), blurRadius: 5, spreadRadius: 1)],
        ),
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(icon, size: 40, color: color),
            const SizedBox(height: 10),
            Text(title, textAlign: TextAlign.center, style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 14)),
          ],
        ),
      ),
    );
  }
}

// 2. ATTENDANCE SCREEN
class AttendanceScreen extends StatelessWidget {
  const AttendanceScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final List<Map<String, String>> attendanceList = [
      {'date': '10 Oct 2026', 'status': 'Present'},
      {'date': '09 Oct 2026', 'status': 'Present'},
      {'date': '08 Oct 2026', 'status': 'Absent'},
      {'date': '07 Oct 2026', 'status': 'Present'},
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('Attendance Records')),
      body: ListView.builder(
        itemCount: attendanceList.length,
        itemBuilder: (context, index) {
          final item = attendanceList[index];
          final bool isPresent = item['status'] == 'Present';
          return Card(
            margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 6),
            child: ListTile(
              leading: Icon(isPresent ? Icons.check_circle : Icons.cancel, color: isPresent ? Colors.green : Colors.red),
              title: Text(item['date']!),
              trailing: Text(item['status']!, style: TextStyle(fontWeight: FontWeight.bold, color: isPresent ? Colors.green : Colors.red)),
            ),
          );
        },
      ),
    );
  }
}

// 3. HOMEWORK SCREEN
class HomeworkScreen extends StatelessWidget {
  const HomeworkScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Daily Homework')),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: const [
          HomeworkCard(subject: 'Mathematics', task: 'Complete Exercise 4.2 questions 1 to 10.', date: 'Due: 12 Oct 2026'),
          HomeworkCard(subject: 'Science', task: 'Read Chapter 5 and write short notes on photosynthesis.', date: 'Due: 13 Oct 2026'),
          HomeworkCard(subject: 'English', task: 'Learn essay writing for weekly test.', date: 'Due: 14 Oct 2026'),
        ],
      ),
    );
  }
}

class HomeworkCard extends StatelessWidget {
  const HomeworkCard({super.key, required this.subject, required this.task, required this.date});
  final String subject, task, date;

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.only(bottom: 12),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.between,
              children: [
                Text(subject, style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 16, color: Colors.blue)),
                Text(date, style: const TextStyle(color: Colors.grey, fontSize: 12)),
              ],
            ),
            const SizedBox(height: 8),
            Text(task, style: const TextStyle(fontSize: 14)),
          ],
        ),
      ),
    );
  }
}

// 4. WEEKLY TEST & MARKS SCREEN
class MarksScreen extends StatelessWidget {
  const MarksScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Weekly Test Marks')),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: const [
          MarksCard(testName: 'Weekly Test #3', subject: 'Maths & Science', marks: '42 / 50', grade: 'A'),
          MarksCard(testName: 'Weekly Test #2', subject: 'English Grammar', marks: '38 / 50', grade: 'B+'),
          MarksCard(testName: 'Weekly Test #1', subject: 'General Science', marks: '45 / 50', grade: 'A+'),
        ],
      ),
    );
  }
}

class MarksCard extends StatelessWidget {
  const MarksCard({super.key, required this.testName, required this.subject, required this.marks, required this.grade});
  final String testName, subject, marks, grade;

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.only(bottom: 12),
      child: ListTile(
        title: Text(testName, style: const TextStyle(fontWeight: FontWeight.bold)),
        subtitle: Text(subject),
        trailing: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.end,
          children: [
            Text(marks, style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 14)),
            Text('Grade: $grade', style: const TextStyle(color: Colors.green, fontWeight: FontWeight.w600)),
          ],
        ),
      ),
    );
  }
}

// 5. NOTICES & HOLIDAYS SCREEN
class NoticeScreen extends StatelessWidget {
  const NoticeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Notices & Holidays')),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: const [
          NoticeCard(title: 'Diwali Holiday Notice', date: 'Oct 20, 2026', description: 'Tuition classes will remain closed from Oct 22 to Oct 26 for Diwali celebrations.'),
          NoticeCard(title: 'Science Exhibition Function', date: 'Oct 30, 2026', description: 'All students must submit their science models by next Wednesday for the exhibition function.'),
        ],
      ),
    );
  }
}

class NoticeCard extends StatelessWidget {
  const NoticeCard({super.key, required this.title, required this.date, required this.description});
  final String title, date, description;

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.only(bottom: 12),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.between,
              children: [
                Text(title, style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 15)),
                Text(date, style: const TextStyle(color: Colors.grey, fontSize: 12)),
              ],
            ),
            const SizedBox(height: 8),
            Text(description, style: const TextStyle(fontSize: 14, color: Colors.black87)),
          ],
        ),
      ),
    );
  }
}

// 6. PAYMENT SCREEN (NEW FEATURE ADDED)
class PaymentScreen extends StatelessWidget {
  const PaymentScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              padding: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Colors.teal[700],
                borderRadius: BorderRadius.circular(12),
              ),
              child: const Row(
                mainAxisAlignment: MainAxisAlignment.between,
                children: [
                  Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text('Pending Fee', style: TextStyle(color: Colors.white70, fontSize: 14)),
                      SizedBox(height: 4),
                      Text('Rs. 1,500', style: TextStyle(color: Colors.white, fontSize: 22, fontWeight: FontWeight.bold)),
                    ],
                  ),
                  Icon(Icons.account_balance_wallet, color: Colors.white, size: 40),
                ],
              ),
            ),
            const SizedBox(height: 20),
            const Text('Pay Tuition Fee', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
            const SizedBox(height: 10),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton.icon(
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.teal,
                  foregroundColor: Colors.white,
                  padding: const EdgeInsets.symmetric(vertical: 12),
                ),
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(content: Text('Redirecting to secure payment gateway (UPI/QR)...')),
                  );
                },
                icon: const Icon(Icons.payment),
                label: const Text('Pay Online Now', style: TextStyle(fontSize: 16)),
              ),
            ),
            const SizedBox(height: 25),
            const Text('Fee Payment History', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
            const SizedBox(height: 10),
            const PaymentHistoryCard(month: 'September 2026', amount: 'Rs. 1,500', date: '05 Sep 2026', status: 'Paid'),
            const PaymentHistoryCard(month: 'August 2026', amount: 'Rs. 1,500', date: '04 Aug 2026', status: 'Paid'),
          ],
        ),
      ),
    );
  }
}

// Wrapper for Navigation Page Route
class PaymentScreenNav extends StatelessWidget {
  const PaymentScreenNav({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Fee Payment')),
      body: const PaymentScreen(),
    );
  }
}

class PaymentHistoryCard extends StatelessWidget {
  const PaymentHistoryCard({super.key, required this.month, required this.amount, required this.date, required this.status});
  final String month, amount, date, status;

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.only(bottom: 10),
      child: ListTile(
        title: Text(month, style: const TextStyle(fontWeight: FontWeight.bold)),
        subtitle: Text('Paid on: $date'),
        trailing: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.end,
          children: [
            Text(amount, style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 14)),
            Text(status, style: const TextStyle(color: Colors.green, fontWeight: FontWeight.w600)),
          ],
        ),
      ),
    );
  }
}

// 7. COMPLAINT SCREEN
class ComplaintScreen extends StatefulWidget {
  const ComplaintScreen({super.key});

  @override
  State<ComplaintScreen> createState() => _ComplaintScreenState();
}

class _ComplaintScreenState extends State<ComplaintScreen> {
  final TextEditingController _controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Text('Submit a Complaint or Feedback', style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold)),
          const SizedBox(height: 10),
          TextField(
            controller: _controller,
            maxLines: 5,
            decoration: const InputDecoration(
              hintText: 'Write your message or complaint here...',
              border: OutlineInputBorder(),
            ),
          ),
          const SizedBox(height: 16),
          ElevatedButton(
            style: ElevatedButton.styleFrom(backgroundColor: Colors.blue[800], foregroundColor: Colors.white),
            onPressed: () {
              if (_controller.text.isNotEmpty) {
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Complaint submitted successfully!')),
                );
                _controller.clear();
              }
            },
            child: const Text('Send to Management'),
          ),
        ],
      ),
    );
  }
}
