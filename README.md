# My-first-project-calculator-
# This is my first project for a course at my university (Habib University). A calculator that demonstrates my expertise in a programming course. 

import tkinter as tk
import math

# Layout: 
root= tk.Tk()
root.title("Calculator")
root.geometry("375x600")
root.resizable(0, 0)

# Text styles:
large_font_size = ("Arial", 32, "bold")
small_font_size = ("Arial", 10)
digits_font_size = ("Arial", 20, "bold")
default_font_size = ("Arial", 14)

# Colors:
offwhite = "#F8FAFF"
white = "#FFFFFF"
light_gray = "#F5F5F5"
light_blue = "#CCEDFF"
label_color = "#25265E"

# Frames:
def create_main_frame():
    main_frame = tk.Frame(root)
    main_frame.pack(expand= True, fill="both")
    return main_frame
main_frame = create_main_frame()

def create_display_frame():
    frame = tk.Frame(main_frame, height= 221, bg= light_gray)
    frame.pack(expand= True, fill= "both")
    return frame
display_frame = create_display_frame()

def create_button_frame():
    bt = tk.Frame(main_frame)
    bt.pack(expand= True, fill= "both")
    return bt
button_frame = create_button_frame()

def create_history_frame():
    global display_frame, history
    history_list = tk.Listbox(display_frame, bg= light_gray, height = 50, width = 100)
    history_list.place(x= 0, y= 0)
    for result in history:
        history_list.insert(tk.END, result)
    history_list.after(10000, history_list.destroy)
# Equation after pressing any operator:
total_expression = ""


#Equation before pressing any operator:
current_expression = ""

#history list:
history = []

# Labels to display frames:
def create_display_labels():
    total_label = tk.Label(display_frame, text= total_expression, anchor= tk.E, bg= light_gray, fg= label_color, padx= 20, font= small_font_size)
    total_label.pack(expand= True, fill= "both")
    
    label = tk.Label(display_frame, text= current_expression, anchor= tk.E, bg= light_gray, fg= label_color, padx= 20, font= large_font_size)
    label.pack(expand= True, fill= "both")
    
    return total_label, label
total_label, label = create_display_labels()
# Operations updating the total label and label:
def update_total_label():
    global total_label, total_expression
    expression = total_expression
    for operator, symbol in operators.items():
        expression = expression.replace(operator, symbol)
    total_label.config(text= expression)

def update_label():
    global current_expression
    current_expression = str(current_expression)
    label.config(text= current_expression[:11])

def add_to_expression(value):
    global current_expression
    current_expression += str(value)
    update_label()

def add_operators(operator):
    global current_expression, total_expression
    current_expression += str(operator)
    total_expression += current_expression
    current_expression = ""
    update_total_label()
    update_label()

def add_power_operator():
    global current_expression
    current_expression += "^"
    update_label()

# Operators
def clear():
    global total_expression, current_expression
    total_expression = ""
    current_expression = ""
    update_total_label()
    update_label()

def evaluate():
    global total_expression, current_expression, history
    total_expression += current_expression
    # for power function:
    if "^" in current_expression:
        n = int(current_expression[:current_expression.find("^")])
        x = int(current_expression[current_expression.find("^")+1::])
        result = power(n,x)
        current_expression = str(result)
        
    try:
        if "^" not in total_expression:
            current_expression = eval(total_expression) 
        result = total_expression + "=" + str(current_expression)
        history.append(result)
        total_expression = ""
    except Exception:
        current_expression = "Error"
    finally:
        update_total_label()
        update_label()

def factorial(n):
    if n == 0:
        return 1
    else:
        return n*(factorial(n-1))

def power(n, x):
    return n**x

def fac_func():
    global current_expression
    result = factorial(int(current_expression))
    current_expression = str(result)
    update_label()

def square():
    global current_expression
    current_expression = str(eval(current_expression + "**2"))
    update_label()

def sqrt():
    global current_expression
    current_expression = str(eval(current_expression + "**0.5"))
    update_label()

def sin_func():
    global current_expression
    expression = float(current_expression)
    current_expression = str(math.sin(expression))
    update_label()    

def cos_func():
    global current_expression
    expression = float(current_expression)
    current_expression = str(math.cos(expression))
    update_label()    

def tan_func():
    global current_expression
    expression = float(current_expression)
    current_expression = str(math.tan(expression))
    update_label()    
        

digits = {
    7:(2,1), 8:(2,2), 9:(2,3),
    4:(3,1), 5:(3,2), 6:(3,3),
    1:(4,1), 2:(4,2), 3:(4,3),
    0:(5,2), ".":(5,1)
}

# Buttons:
def create_histroy_button():
    button = tk.Button(display_frame, text= "H", anchor="w", bg= light_gray, fg=label_color, font= ("Arial", 15, "bold"), borderwidth= 0, command= lambda: create_history_frame())
    button.pack(side= tk.LEFT, padx= 20, pady= 20)

def create_digit_buttons():
    for digit, grid_value in digits.items():
        button = tk.Button(button_frame, text= str(digit), bg= white, fg= label_color, font= digits_font_size, borderwidth= 0, command=lambda x=digit: add_to_expression(x))
        button.grid(row=grid_value[0], column=grid_value[1], sticky= tk.NSEW)
create_digit_buttons()

operators ={"/": "\u00F7", "*":"\u00D7", "-":"-", "+":"+"}
def create_operator_buttons():
    r = 1
    for operator, symbol in operators.items():
        button = tk.Button(button_frame, text= symbol, bg= offwhite, fg= label_color, font= default_font_size, borderwidth= 0, command= lambda x=operator: add_operators(x))
        button.grid(row= r, column= 4, sticky= tk.NSEW)
        r += 1
create_operator_buttons()

def create_fac_button():
    button = tk.Button(button_frame, text= "!", bg= white, fg= label_color, font= default_font_size, borderwidth= 0, command= lambda: fac_func())
    button.grid(row= 5, column= 3, sticky= tk.NSEW)

def create_clear_button():
    button = tk.Button(button_frame, text= "C", bg= offwhite, fg= label_color, font= default_font_size, borderwidth= 0, command= lambda: clear())
    button.grid(row= 0, column= 1, sticky= tk.NSEW)

def create_sin_button():
    button = tk.Button(button_frame, text= "sin", bg= offwhite, fg= label_color, font= default_font_size, border= 0, command= lambda: sin_func())
    button.grid(row= 0, column= 2, sticky= tk.NSEW)

def create_cos_button():
    button = tk.Button(button_frame, text= "cos", bg= offwhite, fg= label_color, font= default_font_size, border= 0, command= lambda: cos_func())
    button.grid(row= 0, column= 3, sticky= tk.NSEW)

def create_tan_button():
    button = tk.Button(button_frame, text= "tan", bg= offwhite, fg= label_color, font= default_font_size, border= 0, command= lambda: tan_func())
    button.grid(row= 0, column= 4, sticky= tk.NSEW)

def create_square_button():
    button = tk.Button(button_frame, text= "x\u00b2", bg= offwhite, fg= label_color, font= default_font_size, borderwidth= 0, command= lambda: square())
    button.grid(row= 1, column= 2, sticky= tk.NSEW)

def create_sqrt_button():
    button = tk.Button(button_frame, text= "\u221ax", bg= offwhite, fg= label_color, font= default_font_size, borderwidth= 0, command= lambda: sqrt())
    button.grid(row= 1, column= 3, sticky= tk.NSEW)

def create_power_button():
    button = tk.Button(button_frame, text= "^", bg= offwhite, fg= label_color, font= default_font_size, borderwidth= 0, command= lambda: add_power_operator())
    button.grid(row= 1, column= 1, sticky= tk.NSEW)

def create_equals_to_button():
    button = tk.Button(button_frame, text= "=", bg= light_blue, fg= label_color, font= default_font_size, borderwidth= 0, command=lambda: evaluate())
    button.grid(row= 5, column= 4, sticky= tk.NSEW)

def create_special_buttons():
    create_histroy_button()
    create_clear_button()
    create_equals_to_button()
    create_fac_button()
    create_sin_button()
    create_cos_button()
    create_tan_button()
    create_square_button()
    create_sqrt_button()
    create_power_button()

create_special_buttons()

# Buttons Frame:
button_frame.rowconfigure(0, weight= 1)
for i in range(1, 5):
    button_frame.rowconfigure(i, weight= 1)
    button_frame.columnconfigure(i, weight= 1)
button_frame.rowconfigure(5, weight=1)

# Main loop:
root.mainloop()

#### Limitations ####
# When calculating power of some number, please add only two numbers. One as a base and other as an exponent. 
# For trignometric functions, add angles first in radians and then press the desired trignometric function.
# The history remains open for 10 secs.
