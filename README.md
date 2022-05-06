# employee-list

app
import React, {Component} from 'react'
import axios from 'axios';
import './App.css';
import EmployeeList from './EmployeeList'


class App extends Component {

  state = {
    employees: []
  }
  
  componentDidMount() {
    axios.get('http://dummy.restapiexample.com/api/v1/employees')
      .then(data => this.setState({employees: data.data.data}))
  }
  
  render() {
    return (
      <div>
        <EmployeeList employees={this.state.employees}/>
      </div>
    )
  }
}

export default App;




-----------------------------

import React, {Component} from 'react'
import Employee from './Employee'

export default class EmployeeList extends Component {
   
    state = {
        searchResult: this.props.employees,
        filter: null
    }
   
    handleChange(e) {
        let searchResult = this.props.employees.filter((item) => {
            if(item.employee_name.toLowerCase().includes(e.target.value)) {
                return item
            }
        })

        this.setState({searchResult: searchResult})
    }

    handleClick() {
        console.log('a')
        if(this.state.filter === null) {
            this.setState({filter: true})
        }
        if(this.state.filter) {
            let filterResult = this.state.searchResult.sort((a, b) => (a.employee_salary < b.employee_salary) ? 1 : -1) 
            this.setState({searchResult: filterResult,filter: false})
        }else {
            let filterResult = this.state.searchResult.sort((a, b) => (a.employee_salary > b.employee_salary) ? 1 : -1) 
            this.setState({searchResult: filterResult,filter: true})
        }
    }
    
    render() {
        return (
            <ul>
                <input onChange={(e) => this.handleChange(e)} placeholder="Ad daxil edin" type='text' />
                <button onClick={() => this.handleClick()}>Salary filter</button>
                <Employee employees={this.state.searchResult}/>
            </ul>
        )
    }
}

---------------------------------------

import React, {Component} from 'react'

export default class Employee extends Component {
    render() {
        return (
                this.props.employees.map((item) => {
                    return (
                        <li key={item.employee_name}>
                        <div>
                            <p>{item.id}</p>
                            <p>{item.employee_name}</p>
                        </div>
                        <div>
                            <p>{item.employee_salary}</p>
                        </div>
                        </li>
                    )
                })
        )
    }
}

----------------------------------------------
.App {
  text-align: center;
}

ul {
  list-style: none;
  padding: 10px 20px;
  width: 700px;
  background-color: blanchedalmond;
  border-radius: 10px;
  box-shadow: 0 0 7px 0px #bdbdbd;
  margin: 0 auto;
}


li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid black;
  border-radius: 5px;
  margin-bottom: 10px;
  padding: 0 10px;
}

li div {
  display: flex;
  align-items: center;
}

li div:first-child p:first-child {
  margin-right: 15px;
  font-weight: 700;
}

input {
  width: 100%;
  margin-bottom: 20px;
  height: 20px;
  font-size: 20px;
}

button {
  margin-bottom: 10px;
}
