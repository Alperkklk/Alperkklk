using System;
using System.Collections.Generic;

// Temel Sınıf (Base Class)
public abstract class Person
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Polymorphism için temel metot
    public abstract void ShowInfo();
}

// Interface
public interface ILogin
{
    bool Login(string username, string password);
}

// Ogrenci Sınıfı
public class Student : Person, ILogin
{
    public string StudentNumber { get; set; }
    public List<Course> EnrolledCourses { get; set; } = new List<Course>();

    public override void ShowInfo()
    {
        Console.WriteLine($"Öğrenci ID: {Id}, Adı: {Name}, Numara: {StudentNumber}");
        Console.WriteLine("Kayıtlı Dersler:");
        foreach (var course in EnrolledCourses)
        {
            Console.WriteLine($" - {course.CourseName} ({course.Credits} kredi)");
        }
    }

    public bool Login(string username, string password)
    {
        return username == "student" && password == "1234"; // Basit doğrulama
    }
}

// OgretimGorevlisi Sınıfı
public class Instructor : Person, ILogin
{
    public string Department { get; set; }
    public List<Course> GivenCourses { get; set; } = new List<Course>();

    public override void ShowInfo()
    {
        Console.WriteLine($"Öğretim Görevlisi ID: {Id}, Adı: {Name}, Bölüm: {Department}");
        Console.WriteLine("Verilen Dersler:");
        foreach (var course in GivenCourses)
        {
            Console.WriteLine($" - {course.CourseName} ({course.Credits} kredi)");
        }
    }

    public bool Login(string username, string password)
    {
        return username == "instructor" && password == "1234"; // Basit doğrulama
    }
}

// Ders Sınıfı
public class Course
{
    public string CourseName { get; set; }
    public int Credits { get; set; }
    public Instructor Instructor { get; set; }
    public List<Student> EnrolledStudents { get; set; } = new List<Student>();

    public void ShowCourseInfo()
    {
        Console.WriteLine($"Ders Adı: {CourseName}, Kredi: {Credits}, Öğretim Görevlisi: {Instructor.Name}");
        Console.WriteLine("Kayıtlı Öğrenciler:");
        foreach (var student in EnrolledStudents)
        {
            Console.WriteLine($" - {student.Name} ({student.StudentNumber})");
        }
    }
}

// Main Program
class Program
{
    static void Main(string[] args)
    {
        // Öğretim Görevlisi Tanımlama
        Instructor instructor = new Instructor { Id = 1, Name = "Dr. Ahmet Yılmaz", Department = "Bilgisayar Mühendisliği" };

        // Ders Tanımlama
        Course course = new Course
        {
            CourseName = "Programlama 101",
            Credits = 4,
            Instructor = instructor
        };
        instructor.GivenCourses.Add(course);

        // Öğrenci Tanımlama
        Student student1 = new Student { Id = 1, Name = "Ali Can", StudentNumber = "2023001" };
        Student student2 = new Student { Id = 2, Name = "Ayşe Kaya", StudentNumber = "2023002" };

        // Öğrencileri Derse Kaydetme
        course.EnrolledStudents.Add(student1);
        course.EnrolledStudents.Add(student2);

        // Öğrencilerin Ders Listesine Eklenmesi
        student1.EnrolledCourses.Add(course);
        student2.EnrolledCourses.Add(course);

        // Bilgilerin Konsolda Gösterimi
        Console.WriteLine("Öğretim Görevlisi Bilgisi:");
        instructor.ShowInfo();

        Console.WriteLine("\nDers Bilgisi:");
        course.ShowCourseInfo();

        Console.WriteLine("\nÖğrenci Bilgisi:");
        student1.ShowInfo();
        student2.ShowInfo();

        Console.ReadLine();
    }
}
