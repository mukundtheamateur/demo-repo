    @PutMapping("/editEmployee")
    public ResponseEntity<EmployeeDTO> editEmployee(@Valid @RequestBody Employee employee) {
         log.info("Editing employee: {}", employee.getEmployeeId());
        return ResponseEntity.ok(employeeService.editEmployee(employee));
    }

@Transactional
    public EmployeeDTO editEmployee(Employee employee) {
        if (!employeeRepository.existsById(employee.getEmployeeId())) {
            throw new EmployeeNotFoundException("Employee not found with ID: " + employee.getId());
        }
        Employee existingEmployee = employeeRepository.findByEmployeeId(employee.getEmployeeId()).orElseThrow(()-> new EmployeeNotFoundException("Employee not found"));
        existingEmployee.setName(employee.getName());
        existingEmployee.setAge(employee.getAge());
        existingEmployee.setDepartment(employee.getDepartment());
        Employee savedEmployee = employeeRepository.save(existingEmployee);

        return new EmployeeDTO(savedEmployee);
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

PUT http://localhost:8080/employee/editEmployee
request body: 
{
    "id": 1,
    "employeeId": 123456,
    "name": "Mukund Shukla",
    "age": 24,
    "department": {
        "id": 1
    }
}
Response: 
Employee not found with ID: 1

please fix the issues as employee with id 1 exists in the table

