# c4c_split_error
 String Find Function: Importing parameter Length is not valid.


	import AP.Common.GDT; //for split	

 	var deliveryLocalString = "1 ~~ 2  ~~ 3  ~~ 4  ~~ 5  ~~ 6  ~~ 7\n" 
		+
		"11 ~~ 22 ~~ 33 ~~ 44 ~~ 55 ~~ 66 ~~ 77\n"
		+
		"111 ~~ 222 ~~ 333 ~~ 444 ~~ 555 ~~ 666 ~~ 777\n"
		+
		"1111 ~~ 2222 ~~ 3333 ~~ 4444 ~~ 5555 ~~ 6666 ~~ 7777\n"
		+
		"11111 ~~ 22222 ~~ 33333 ~~ 44444 ~~ 55555 ~~ 66666 ~~ 77777\n"
		+
		"111111 ~~ 222222 ~~ 333333 ~~ 444444 ~~ 555555 ~~ 666666 ~~ 777777\n";
  
	var deliveryStringTable : collectionof DataType::LANGUAGEINDEPENDENT_EXTENDED_Text; //if it doesn't work try to replace with a string or LongText
        //       ^ this is the FINAL result of split!
	//****************************************************
	//first we have a very long string, which we have to split to create a table.
	//End of line as delimeter
	var split_at = "\n"; //your delimiter, cloud be anything e.g. "\n"
	var length_at = split_at.Length(); //Length()  doesn't count white-spaces, so be carefull with splitting something using whitespace delimetters e.g. " ~~ "

	//go through the string and split it
	var start = 0;
	while (true)
	{
		// check if start is out of bounds
		// find next match
		var pos = deliveryLocalString.Find(split_at, start);

		// no further occurence found?
		if (pos == -1)
		{
		
			// check if any characters remain at end of string
			if (deliveryLocalString.Length() > start)
			{
				var remainder = deliveryLocalString.Substring(start);
				deliveryStringTable.Add(remainder);
			}

			// abort loop
			break;
		}

		// otherwise add found string to newlineParsedTab
		var sub_length = pos - start;
		var substring = deliveryLocalString.Substring(start, sub_length);
		deliveryStringTable.Add(substring);

		// set new start position for next loop
		start = pos + length_at;
	}
 
 above code sometimes will have an error like this: String Find Function: Importing parameter Length is not valid.
 
 ![image](https://user-images.githubusercontent.com/56394602/147851820-4bbaedd2-c272-4c30-bb92-d921fb48edc0.png)
 
 the reason why - last line of my deliveryLocalString contains delimeter and nothing else		"111111 ~~ 222222 ~~ 333333 ~~ 444444 ~~ 555555 ~~ 666666 ~~ 777777\n";


 so I would suggest to add below check -
 
 	if (deliveryLocalString.EndsWith(split_at))
	{
		var deliveryLocalStringLength = deliveryLocalString.Length();
		deliveryLocalString = deliveryLocalString.Substring(0, deliveryLocalStringLength - length_at);
	}
