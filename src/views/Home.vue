<template>
   <div class = 'flex bg-green-500 min-h-screen'>
         <div class = 'bg-red-200 w-1/2'>
              <h1 class = 'text-2xl p-2 font-bold'>Enter your grades for the subject here.</h1>
              <div v-for = "(item, index) of data" :key = 'item.id'>
                <span class = 'flex'>
                    <input
                       type = 'number'
                       :placeholder = "' Grade ' + (index + 1)"
                       class = 'p-2 m-2 w-1/2'
                        v-model = 'item.grade'
                    > 
                    <input 
                       type = 'number'
                       placeholder = "Weight in % (exclude the '%' symb))"
                       class = 'p-2 m-2 w-1/2'
                       v-model = 'item.weight'
                    >
                
                   <select v-model = 'item.category' class = 'p-2 m-2 w-1/2'> 
                        <option value = "tests">Tests</option>
                        <option value = "quizzes">Quizzes</option>
                        <option value = "classwork">Classwork</option>
                        <option value = "homework">Homework</option>
                        <option value = "quarterlies">Quarterlies</option>
                    </select> 
               </span>
             </div>
             <button type = 'button' @click = 'calculate()' class = 'p-2 m-2 bg-blue-700 text-white rounded shadow-lg'>Calculate</button>
             <button type = 'button' @click = 'addCourse()' class = 'p-2 m-2 bg-green-500 text-white rounded shadow-lg'>Add Course</button>
             <button type = 'button' @click = 'removeCourse()' class = 'p-2 m-2 bg-red-500 text-white rounded shadow-lg'>Remove Course</button>
             <button type = 'button' @click = 'clear()' class = 'p-2 m-2 bg-yellow-300 rounded shadow-lg'>Clear</button>
         </div>
         <div class = 'bg-blue-200 w-1/2'> 
             <h1 class = 'm-2 p-2 text-xl font-bold'>Results</h1>
             
              <b class = 'text-4xl'>Average</b> <h1 class = 'sm:text-9xl text-5xl mb-20'><b>{{ finalGrade }}</b>%</h1>
            
              <b class = 'text-4xl'>Grade</b> <h1 :class = "color" class = 'text-9xl mb-10'><b>{{ grade || "??"}}</b></h1>

              <h1 class = ''>To use this calculator, first input the fields requested. The Grade Category is used to put all your grades for the Marking Period. The Weight is the percent that the grade carries. Finally, you must organize the grades with the drop down. Then you will recieve your result!</h1>
         </div> 
   </div>
</template>

<script>
export default {
      data() { 
          return { 
               finalGrade: 0,
               grade: "",
               color: "", 
               data: [
               { 
                   id: 1,
                   grade: "", 
                   weight: "", 
                   category: ""
               },
               { 
                   id: 2,
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 3, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 4, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 5, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 6, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 7, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 7, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 8, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }
               ]
          }
      }, 
      methods: { 
          addCourse() { 
              this.data.push({ 
                  grade: "", 
                  weight: ""
              })
          }, 
          removeCourse() { 
              this.data.pop(); 
          }, 
          calculate() { 
               let data = this.data; 
               let redFlags = 0; 
               let problemArray = [];
                for (let item of data) { 
                    if ((item.grade < 0) || (item.weight < 0)) { 
                        redFlags++; 
                        problemArray.push(item.id); 
                    }
                }

                if (redFlags != 0) return alert("ALERT: You need to replace " + redFlags + ` of your fields: \n LOCATION: FIELD ${problemArray.toString()}` );

                function calculateScore(category) { 
                    let score = 0; 
                    let scores = data.filter(item => { 
                       return item.category === category; 
                    })
                if (scores.length < 1) return score; 

                   let { weight } = data.find(item => { 
                        return item.category === category; 
                    })
                     
                    let average = 0; 
                    for (let item of scores) { 
                        average = parseFloat(average) + parseFloat(item.grade); 
                    }
                    score = (average / scores.length); 


                    score = (score * (weight / 100));
                  

                            return score; 
              }

          function weight(category) {
                 let findWeight = data.find(item => {
                     return item.category === category;
                 })
                 if (!findWeight) {
                    return 0;
                 }
                 else {
                    return parseFloat(findWeight.weight);
                 }
            }

              let totalScore = calculateScore("tests") + calculateScore("quizzes") + calculateScore("classwork") + calculateScore("homework"); 
              
              let totalWeight = weight("tests") + weight("quizzes") + weight("classwork") + weight("homework"); 

              let final = (totalScore * 100) / totalWeight; 
                
            this.finalGrade = (final.toFixed(1) + '0'); 

            if (final >= 96.5) { 
                  this.grade = "A+"
                  this.color = "text-green-600"
            }
            else if (final >= 92.5) { 
                 this.grade = "A"
                 this.color = "text-green-500";
            }
            else if (final >= 89.5) { 
                this.grade = "A-"
                this.color = "text-green-400"
            }
            else if (final >= 86.5) { 
                this.grade = "B+"
                this.color = "text-green-400"
            }
            else if (final >= 82.5) { 
                this.grade = "B"
                this.color = "text-green-300"
            }
            else if (final >= 79.5) { 
                this.grade = "B-"
                this.color = "text-green-300"
            }
            else if (final >= 76.5) { 
                this.grade = "C+"
                this.color = "text-yellow-300"
            }
            else if (final >= 72.5) { 
                this.grade = "C"
                this.color = "text-yellow-500"
            }
            else if (final >= 69.5) { 
                this.grade = "C-"
                this.color = "text-yellow-600"
            }
            else if (final >= 65) { 
                this.grade = "D"
                this.color = "text-red-300"
            }
            else { 
                this.grade = "F"
                this.color = "text-red-600"
            }
            
          }, 
          clear() { 
              this.data =  [
               { 
                   id: 1,
                   grade: "", 
                   weight: "", 
                   category: ""
               },
               { 
                   id: 2,
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 3, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 4, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 5, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 6, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 7, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 7, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }, 
               { 
                   id: 8, 
                   grade: "", 
                   weight: "", 
                   category: ""
               }
               ]; 
          }

      }

}
</script>

<style>

</style>