# 📅 Best Timetable Generator for NTU Students  


## 🔍 Business Problem  

### 1.1 Background & Problem  
At NTU, first-year students are given fixed timetables, while upper-year students use the **STARS system** to register. This process often leads to **inefficient timetables**:  
- Back-to-back lessons without breaks  
- Long idle gaps (e.g., 8:30 AM class, then 4:30 PM class)  
- Conflicts with **CCAs**（Co-Curricular Activities）, part-time jobs, or personal commitments  

Without an optimization tool, students spend hours trying to fix schedules manually.  

### 1.2 Who It Affects  
- All NTU students  
- Especially those balancing **CCAs, part-time jobs, and heavy workloads**  

### 1.3 How It Impacts Students  
- Long wasted hours on campus  
- Poor work-life balance  
- Physical & mental fatigue due to back-to-back lessons  

### 1.4 Current Solutions  
- Pre-planning and index-grabbing during STARS registration  
- Swapping indices during add/drop period  
- **Still inefficient and time-consuming**  

### 1.5 Relevant Assumptions  
- All lessons occur weekly, no odd/even weeks  
- Fixed 1-hour slots  

---

## 💡 Our Solution  

We developed a **Best Timetable Generator** using optimization methods.  

- **Objective:** Minimize (a) number of days on campus, (b) total time spent on campus  
- Students can customize preferences (prioritize fewer days vs. shorter time)  
- Supports blocking out unavailable times (e.g., "no Friday evening classes")  
- Outputs **top 3 optimized timetables** instantly  

Currently tested on **Year 2 Business (BA) modules**, but extendable to all NTU students.  

---

## ⚙️ Model Setting  

### 3.1 Data Preparation & Cleaning  
- Source: NTU official scheduling portal (scraped)  
- Preprocessed with Python to clean inconsistent formatting  
- Standardized into structured table format with:  
  - `Course Code` | `Index` | `Type` | `Group` | `Day` | `Time`  

### 3.2 Model Formulation  
**Objective Function:**  
Minimize:  
- `α × (number of days on campus)`  
- `β × (total time on campus)`  

**Constraints:**  
1. **Course Index Selection** → Only one index per course  
2. **No Overlaps** → No two classes overlap in time  
3. **Active Day Tracking** → If a class is chosen, that day is active  
4. **Total Time Calculation** → Track earliest and latest class per day  
5. **Blocked Time** → No classes in unavailable slots  
6. **Max Consecutive Hours** → Limit back-to-back lessons  

---

## 🖥️ Computational Results  

### 3.3.1 User Inputs  
Students specify:  
- **Max consecutive hours** (slider)  
- **Blocked times** (customizable)  
- **Priority sliders** (days vs. time on campus)  
- **Number of solutions** to generate  

Weights `α` and `β` are scaled to balance priorities.  

### 3.3.2 Outputs  
1. **Optimized Timetables** → Weekly schedule (Mon–Fri, 08:30–21:30)  
2. **Summary Stats** →  
   - Active days  
   - Hours per day  
   - Total weekly hours  

If no feasible solution exists, the system explains the infeasibility.  

---

## 📊 Analysis of Solution  

### 4.1 Limitations  
- **Dependent on STARS** (no real-time integration yet)  
- **Doesn’t account for demand** (popular indices may fill up instantly)  
- **Timetable data changes each term** (manual updates needed)  
- **Exam schedules not included**  

### 4.2 Recommendations  
- Add **demand weighting** based on historical index fill rates  
- Enable **real-time availability input**  
- Automate scraping/updating of NTU data  
- Add **exam scheduling constraints**  
- Provide **soft constraints** (e.g., suggest relaxing blocked times instead of infeasible error)  

---

## ✅ Conclusion  

The **Best Timetable Generator** simplifies NTU students’ planning process by:  
- Saving hours of manual searching  
- Reducing stress during course registration  
- Creating timetables tailored to personal needs  

Future improvements will focus on:  
- **Integration with STARS**  
- **Real-time availability checks**  
- **Mobile app deployment**  

Ultimately, this project aims to help students **win the "STARS War"** with less stress and better time management.  

---

## 📚 References  

- NTU. (n.d.). Class Schedule.  
  [https://wish.wis.ntu.edu.sg/webexe/owa/aus_schedule.main](https://wish.wis.ntu.edu.sg/webexe/owa/aus_schedule.main)  

---

## 📎 Appendix  

### Appendix A  
- Figures: Preprocessing steps, UI snapshots, optimized outputs  

### Appendix B  
**Decision Variables & Parameters:**  
- `xi` = binary, index selection  
- `yd` = binary, active day  
- `z_startd` / `z_endd` = earliest & latest class per day  
- `z_totald` = total duration  
- `classd,t` = binary slot indicator  
- Parameters: `α`, `β`, blocked times, max consecutive hours  
