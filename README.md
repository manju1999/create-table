package main

import (
	"fmt"
	"os"
	"github.com/ziutek/mymysql/mysql"
	_ "github.com/ziutek/mymysql/native" // Native engine
	// _ "github.com/ziutek/mymysql/thrsafe" // Thread safe engine
)

func main() {

	// Database connection
	db := mysql.New("tcp", "", "127.0.0.1:3306","root" ,"superuser", "mysql")

	err := db.Connect()
	if err != nil {
		panic(err)
	}else{
fmt.Println("connected")
}

// Create table 
_, err = db.Start("CREATE TABLE student (name VARCHAR(20), school VARCHAR(20))")
//checkError(err)

// Insert the table values
res, err := db.Start("insert into student values ('Ram', 'BVR')")
//checkError(err)

if err != nil {
	panic(err)
}
// Print fields names
for _, field := range res.Fields() {
	fmt.Print(field.Name, " ")
}
fmt.Println()

// Print all rows
for {
	row, err := res.GetRow()
		//checkError(err)
		if err != nil {
		//	panic(err)
		}

		if row == nil {
			// No more rows
			break
		}

	// Print all cols
	for _, col := range row {
		if col == nil {
			fmt.Print("<NULL>")
		} else {
			os.Stdout.Write(col.([]byte))
		}
		fmt.Print(" ")
	}

	fmt.Println()
	

}

}



