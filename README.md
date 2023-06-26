# Vba-ChallengeNew

The exercise YearStockData looks for the performance of all the stocks during the years of 2018, 2019 and 2020.
The records are sorted by date.

There are two Functions: 
   -StocksRoutine: which does all the calculations related to the exercise
   -ClearColumns:  which reset all the columns used to print the results of the calculations and required formatting

Sub StocksRoutine()
   'look through the sheets
   Dim ws As Worksheet, i As Long, j As Long
   Dim TotalVolume As Double
   
   Dim GreatestIncrease As Double
   Dim GreatestDecrease As Double
   Dim GreatestVolume As Double
   Dim PercentageChange As Double
   
   For Each ws In Sheets
       GreatestVolume = 0 'initializes for the next sheet
       j = 0
       TotalVolume = 0
       lastRow = ws.Cells(Rows.Count, "A").End(xlUp).Row
       
       GreatestIncrease = 0
       GreatestDecrease = 0
       GreatestVolume = 0
       
       TickerIncrease = ""
       TickerDecrease = ""
       TickerGreatestVolume = ""
  
       'headers
       ws.Range("I" & 1).Value = "Ticker"
       ws.Range("J" & 1).Value = "Yearly Change"
       ws.Range("K" & 1).Value = "Percent Change"
       ws.Range("L" & 1).Value = "Volume"
       
     
       
       openingPrice = ws.Cells(2, 3).Value 'for the first stock
       
       For i = 2 To lastRow
          
          If ws.Cells(i, 1).Value <> ws.Cells(i + 1, 1).Value Then
             
             'print results
             ws.Range("I" & 2 + j).Value = ws.Cells(i, 1).Value
             
             'yearly change=closingPrice -openingPrice
             ws.Range("J" & 2 + j).Value = ws.Cells(i, 6).Value - openingPrice
             
             'Percent Change
             PercentageChange = (ws.Cells(i, 6).Value - openingPrice) / openingPrice
             ws.Range("K" & 2 + j).Value = Format((ws.Cells(i, 6).Value - openingPrice) / openingPrice, "###,###.##%")

             If (ws.Cells(i, 6).Value < openingPrice) Then
                'decrease
                ws.Cells(2 + j, 10).Interior.ColorIndex = 3
                If PercentageChange < GreatestDecrease Then
                   GreatestDecrease = PercentageChange
                   TickerDecrease = ws.Cells(i, 1).Value
                End If
                
             Else
                'increase
                ws.Cells(2 + j, 10).Interior.ColorIndex = 4
                If PercentageChange > GreatestIncrease Then
                   GreatestIncrease = PercentageChange
                   TickerIncrease = ws.Cells(i, 1).Value
                End If
                
             End If
             
              
             'volume
             TotalVolume = TotalVolume + ws.Cells(i, 7).Value
             ws.Range("L" & 2 + j).Value = TotalVolume
             
             'increment variables
             j = j + 1
             TotalVolume = 0
                              
             openingPrice = ws.Cells(i + 1, 3).Value
          Else
            TotalVolume = TotalVolume + ws.Cells(i, 7).Value

          End If
          'Greatest Volume
          If TotalVolume > GreatestVolume Then
             GreatestVolume = TotalVolume
             TickerVolume = ws.Cells(i, 1).Value
             End If

       Next i
       'extra activity
       ws.Cells(2, 15).Value = "Greatest % Increase"
       ws.Cells(3, 15).Value = "Greatest % Decrease"
       ws.Cells(4, 15).Value = "Greatest Total Volume"
       ws.Cells(1, 16).Value = "Ticker"
       ws.Cells(1, 17).Value = "Value"
       
       ws.Cells(2, 16).Value = TickerIncrease
       ws.Cells(3, 16).Value = TickerDecrease
       ws.Cells(4, 16).Value = TickerVolume
       
       ws.Cells(2, 17).Value = Format(GreatestIncrease, "###,###.##%") 'GreatestIncrease
       ws.Cells(3, 17).Value = Format(GreatestDecrease, "###,###.##%") 'GreatestDecrease
       ws.Cells(4, 17).Value = GreatestVolume
       
       
   Next ws
   
End Sub
Sub ClearColumns()
      Dim ws As Worksheet
   For Each ws In Sheets
       ws.Range("I1", "I200000").ClearContents
       ws.Range("J1", "J200000").ClearContents
       ws.Range("J1", "J200000").Interior.ColorIndex = 0
       ws.Range("K1", "K200000").ClearContents
       ws.Range("L1", "L200000").ClearContents
       ws.Range("M1", "M200000").ClearContents
       ws.Range("N1", "N200000").ClearContents
       ws.Range("O1", "O200000").ClearContents
       ws.Range("P1", "P200000").ClearContents
       ws.Range("Q1", "Q200000").ClearContents
   Next ws
End Sub
