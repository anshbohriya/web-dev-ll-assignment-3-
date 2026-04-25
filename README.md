# web-dev-ll-assignment-3-
import React, { useState } from "react";
import "./App.css";

function Header() {
  return (
    <header className="header">
      <h1>Student Scoreboard</h1>
    </header>
  );
}

function StudentRow({ student, index, onUpdateScore }) {
  const isPass = student.score >= 40;

  return (
    <tr>
      <td>{index + 1}</td>
      <td>{student.name}</td>
      <td>
        <input
          type="number"
          value={student.score}
          onChange={(e) =>
            onUpdateScore(index, Number(e.target.value))
          }
          className="score-input"
        />
      </td>
      <td className={isPass ? "pass" : "fail"}>
        {isPass ? "Pass" : "Fail"}
      </td>
    </tr>
  );
}

function StudentTable({ students, onUpdateScore }) {
  return (
    <table className="student-table">
      <thead>
        <tr>
          <th>S.No</th>
          <th>Student Name</th>
          <th>Score</th>
          <th>Status</th>
        </tr>
      </thead>

      <tbody>
        {students.map((student, index) => (
          <StudentRow
            key={index}
            student={student}
            index={index}
            onUpdateScore={onUpdateScore}
          />
        ))}
      </tbody>
    </table>
  );
}

function AddStudentForm({ onAddStudent }) {
  const [name, setName] = useState("");
  const [score, setScore] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();

    if (name.trim() === "" || score === "") {
      return;
    }

    onAddStudent({
      name: name.trim(),
      score: Number(score),
    });

    setName("");
    setScore("");
  };

  return (
    <form className="student-form" onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Enter student name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />

      <input
        type="number"
        placeholder="Enter score"
        value={score}
        onChange={(e) => setScore(e.target.value)}
      />

      <button type="submit">Add Student</button>
    </form>
  );
}

export default function App() {
  const [students, setStudents] = useState([
    { name: "Ansh", score: 78 },
    { name: "Vaibhav", score: 35 },
    { name: "Asitya", score: 52 },
  ]);

  const updateScore = (index, newScore) => {
    const updatedStudents = [...students];
    updatedStudents[index].score = newScore;
    setStudents(updatedStudents);
  };

  const addStudent = (newStudent) => {
    setStudents([...students, newStudent]);
  };

  return (
    <div className="app-container">
      <Header />
      <AddStudentForm onAddStudent={addStudent} />
      <StudentTable
        students={students}
        onUpdateScore={updateScore}
      />
    </div>
  );
}
