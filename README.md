package com.cts.assessment.controller;

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
