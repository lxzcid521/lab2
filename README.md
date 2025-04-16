


import java.lang.Exception

// Custom exception
class InvalidGradeException(message: String) : Exception(message)

fun main() {
    println("Enter the number of students:")
    val n = readLine()!!.toIntOrNull() ?: 0
    val names = Array<String>(n) { "" }
    val grades = Array<Int>(n) { 0 }
    var i = 0
    while (i < n) {
        println("Enter the name of student #${i + 1} (can be null):")
        val inputName = readLine()
        val name = inputName?.takeIf { it.isNotBlank() } ?: "Unknown"
        names[i] = name
        try {
            println("Enter the grade of student $name (0–100):")
            val grade = readLine()!!.toInt()
            if (grade !in 0..100) {
                throw InvalidGradeException("Grade must be between 0 and 100.")
            }
            grades[i] = grade
            i++ // Move to the next student only if no exception was thrown
        } catch (e: InvalidGradeException) {
            println("Error: ${e.message}. Please re-enter.")
        } catch (e: Exception) {
            println("Invalid input. Please try again.")
        }
    }
    // Calculate statistics
    val average = grades.average()
    val max = grades.maxOrNull()!!
    val min = grades.minOrNull()!!
    println("\n--- Results ---")
    println("Average grade: %.2f".format(average))
    println("Highest grade: $max")
    println("Lowest grade: $min")
    println("\nHonor students (grade ≥ 90):")
    for (j in grades.indices) {
        if (grades[j] >= 90) {
            println("${names[j]} — ${grades[j]}")
        }
    }
    // Comment based on average grade
    val comment = when {
        average >= 90 -> "High level"
        average in 70.0..89.99 -> "Medium level"
        else -> "Low level"
    }
    println("\nComment: $comment")
}
