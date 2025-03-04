package com.cts.assessment.controller;

org.hibernate.StaleObjectStateException: Row was updated or deleted by another transaction (or unsaved-value mapping was incorrect): [com.cts.assessment.entity.Department#1]

package com.cts.assessment.entity;

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
    private Long id;

    @NotBlank(message = "Employee name is required")
    private String name;

    @Min(value = 18, message = "Age must be at least 18")
    private int age;

    @ManyToOne
    @JoinColumn(name = "department_id", nullable = false)
    private Department department;

}

package com.cts.assessment.entity;

import jakarta.persistence.*;
import lombok.*;

import java.util.List;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@Getter
@Setter
public class Department {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;

    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    private List<Employee> employees;
}




import com.cts.assessment.entity.Employee;
import com.cts.assessment.service.EmployeeService;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import java.util.Arrays;
import java.util.Optional;

import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(EmployeeController.class)
class EmployeeControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private EmployeeService employeeService;

    @Autowired
    private ObjectMapper objectMapper;

    private Employee employee;

    @BeforeEach
    void setUp() {
        employee = new Employee();
        employee.setId(123456L);
        employee.setName("John Doe");
        employee.setDepartmentId(10L);
    }

    @Test
    void testGetEmployee_ValidId_ShouldReturnEmployee() throws Exception {
        when(employeeService.getEmployeeById(123456L)).thenReturn(employee);

        mockMvc.perform(get("/employee/getEmployee/123456"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("John Doe"));
    }

    @Test
    void testGetEmployee_InvalidId_ShouldReturn400() throws Exception {
        mockMvc.perform(get("/employee/getEmployee/1234")) // Invalid ID (not 6 digits)
                .andExpect(status().isBadRequest());
    }

    @Test
    void testGetEmployeeByDepartment_ValidDepartmentId_ShouldReturnEmployees() throws Exception {
        when(employeeService.getEmployeesByDepartmentId(10L)).thenReturn(Arrays.asList(employee));

        mockMvc.perform(post("/employee/getEmployeeByDepartmentId")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(10L)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.length()").value(1));
    }
}
