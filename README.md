    public EmployeeDTO editEmployee(Employee employee) {
        if (!employeeRepository.existsById(employee.getId())) {
            throw new EmployeeNotFoundException("Employee not found with ID: " + employee.getId());
        }
        
        return new EmployeeDTO(employeeRepository.save(employee));
    }

package com.cts.assessment.dto;

import com.cts.assessment.entity.Employee;
import lombok.Data;

@Data
public class EmployeeDTO {
    private int employeeId;
    private String name;
    private int age;
    private String departmentName;

    //constructor
    public EmployeeDTO(Employee employee){
        this.employeeId = employee.getEmployeeId();
        this.age = employee.getAge();
        this.name = employee.getName();
        this.departmentName = employee.getDepartment().getName();
    }
}

package com.cts.assessment.entity;

import com.fasterxml.jackson.annotation.JsonBackReference;
import jakarta.persistence.*;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;
import lombok.*;


@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor

public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private int employeeId;

    @NotBlank(message = "Employee name is required")
    private String name;

    @Min(value = 18, message = "Age must be at least 18")
    private int age;

    @ManyToOne
    @JoinColumn(name = "department_id", nullable = false)
    @JsonBackReference
    private Department department;

}

please fix my editEmployee method, I need to use Put mapping and need to update the employee
