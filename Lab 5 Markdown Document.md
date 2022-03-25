# Lab 5 Markdown Document

#### Max Miller

[Here](markdownguide.org) is the markdown guide used to create this document.

## Tasks

In this lab, we were tasked with turning WET code into DRY code. In order to accomplish this, three parameters must be fullfilled:
* Is repeated code reduced as much as possible?
* Have all magic numbers (floating constants) been removed or assigned to a variable?
* Are functions and classes used whenever applicable?

If these parameters have all been met, then we can successfully say we have DRYed out the code.

## Process

The file we were tasked with DRYing out was a simple transaction recorder. It could be used to keep track of the balances of a saving and a checking account. Here is the full, WET code:
    
    from typing import Tuple


    def saturdays_bank_transactions(transations) -> Tuple[float, float]:
        savings = 1096.25
        checking = 1590.80

        checking += (transations[0] * 0.85)
        savings += (transations[0] * 0.15)
    
        checking += transations[1]
    
        checking += transations[2]
    
        checking += transations[3]

        checking += (transations[4] * 0.85)
        savings += (transations[4] * 0.15)
    
        checking += (transations[5] * 0.85)
        savings += (transations[5] * 0.15)

        checking += transations[6]
    
        checking += transations[7]
    
        checking += transations[8]
    
        checking += transations[9]
    
        checking += transations[10]

    return checking, savings

    if __name__ == "__main__":
        transations = [300.00, -50.00, -5.00, -20, 15.72, 2083.93, -1034.00, -420.00, -5.23, -15.93, -72.90]
        new_balance = saturdays_bank_transactions(transations)
        print("Your new checking balance is:", '${:.2f}'.format(round(new_balance[0], 2)), "\nYour new savings balance is: ", '${:.2f}'.format(round(new_balance[1], 2)))

As you can see, this code is sopping WET. The first problem that immediately jumped out at me was solving the issue of the iteration of the addition assignment line for each individual transaction. Considering these lines are identical, save for the fact that each references the next object in a list, a for loop can easily solve this WETness. I replaced the addition assignment lines inside the saturday_bank_transactions function with a for loop:

    for item in transations:
            if item <= 0:
                checking += item
            else:
                checking += (item * 0.85)
                savings += (item * 0.15)
                
Next, I realized that I was still using magic numbers (`0.85` and `0.15`) in my code. For clarity and ease of future reference, I assigned these magic numbers to two variables inside the function, and replaced them with the respective variables inside the for loop:
    
    def saturdays_bank_transactions(transations) -> Tuple[float, float]:
        savings = 1096.25
        checking = 1590.80
        CHECKING_PROPORTION = 0.85
        SAVINGS_PROPORTION = 0.15
    
        for item in transations:
            if item <= 0:
                checking += item
            else:
                checking += (item * CHECKING_PROPORTION)
                savings += (item * SAVINGS_PROPORTION)

    return checking, savings

Next, I scanned the code and made sure functions and classes were used whenever applicable. They are, so I would consider this code to be DRY. Following is the full DRY code:

    from typing import Tuple


    def saturdays_bank_transactions(transations) -> Tuple[float, float]:
        savings = 1096.25
        checking = 1590.80
        CHECKING_PROPORTION = 0.85
        SAVINGS_PROPORTION = 0.15
    
        for item in transations:
            if item <= 0:
                checking += item
            else:
                checking += (item * CHECKING_PROPORTION)
                savings += (item * SAVINGS_PROPORTION)

    return checking, savings

    if __name__ == "__main__":
        transations = [300.00, -50.00, -5.00, -20, 15.72, 2083.93, -1034.00, -420.00, -5.23, -15.93, -72.90]
        new_balance = saturdays_bank_transactions(transations)
        print("Your new checking balance is:", '${:.2f}'.format(round(new_balance[0], 2)), "\nYour new savings balance is: ", '${:.2f}'.format(round(new_balance[1], 2)))