---
layout: post
title: "use protobuf"
description: ""
category: program 
tags: [c++, protobuf]
---

#### checkout protobuf

	svn checkout http://protobuf.googlecode.com/svn/trunk protobuf

#### compile, install
	
	[dennis@localhost protobuf]$ cd protobuf
	[dennis@localhost protobuf]$ sh autogen.sh 
	[dennis@localhost protobuf]$ ./configure
	[dennis@localhost protobuf]$ make check 
	[dennis@localhost protobuf]$ make install

	[dennis@localhost protobuf]$ protoc --version
	libprotoc 2.5.1

#### write sample code

	[dennis@localhost protobuf]$ mkdir ../google-code-sample
	[dennis@localhost protobuf]$ cd ../google-code-sample
	[dennis@localhost google-code-sample]$ vim addressbook.proto
	[dennis@localhost google-code-sample]$ vim pb_write.cc
	[dennis@localhost google-code-sample]$ vim pb_read.cc
	[dennis@localhost google-code-sample]$ vim Makefile

#### cat addressbook.proto

	package tutorial;

	message Person {
	  required string name = 1;
	  required int32 id = 2;
	  optional string email = 3;

	  enum PhoneType {
		MOBILE = 0;
		HOME = 1;
		WORK = 2;
	  }

	  message PhoneNumber {
		required string number = 1;
		optional PhoneType type = 2 [default = HOME];
	  }

	  repeated PhoneNumber phone = 4;
	}

	message AddressBook {
	  repeated Person person = 1;
	}

#### cat pb_write.cc

	#include <iostream>
	#include <fstream>
	#include <string>
	#include "addressbook.pb.h"
	using namespace std;

	// This function fills in a Person message based on user input.
	void PromptForAddress(tutorial::Person* person) {
	  cout << "Enter person ID number: ";
	  int id;
	  cin >> id;
	  person->set_id(id);
	  cin.ignore(256, '\n');

	  cout << "Enter name: ";
	  getline(cin, *person->mutable_name());

	  cout << "Enter email address (blank for none): ";
	  string email;
	  getline(cin, email);
	  if (!email.empty()) {
		person->set_email(email);
	  }

	  while (true) {
		cout << "Enter a phone number (or leave blank to finish): ";
		string number;
		getline(cin, number);
		if (number.empty()) {
		  break;
		}

		tutorial::Person::PhoneNumber* phone_number = person->add_phone();
		phone_number->set_number(number);

		cout << "Is this a mobile, home, or work phone? ";
		string type;
		getline(cin, type);
		if (type == "mobile") {
		  phone_number->set_type(tutorial::Person::MOBILE);
		} else if (type == "home") {
		  phone_number->set_type(tutorial::Person::HOME);
		} else if (type == "work") {
		  phone_number->set_type(tutorial::Person::WORK);
		} else {
		  cout << "Unknown phone type.  Using default." << endl;
		}
	  }
	}

	// Main function:  Reads the entire address book from a file,
	//   adds one person based on user input, then writes it back out to the same
	//   file.
	int main(int argc, char* argv[]) {
	  // Verify that the version of the library that we linked against is
	  // compatible with the version of the headers we compiled against.
	  GOOGLE_PROTOBUF_VERIFY_VERSION;

	  if (argc != 2) {
		cerr << "Usage:  " << argv[0] << " ADDRESS_BOOK_FILE" << endl;
		return -1;
	  }

	  tutorial::AddressBook address_book;

	  {
		// Read the existing address book.
		fstream input(argv[1], ios::in | ios::binary);
		if (!input) {
		  cout << argv[1] << ": File not found.  Creating a new file." << endl;
		} else if (!address_book.ParseFromIstream(&input)) {
		  cerr << "Failed to parse address book." << endl;
		  return -1;
		}
	  }

	  // Add an address.
	  PromptForAddress(address_book.add_person());

	  {
		// Write the new address book back to disk.
		fstream output(argv[1], ios::out | ios::trunc | ios::binary);
		if (!address_book.SerializeToOstream(&output)) {
		  cerr << "Failed to write address book." << endl;
		  return -1;
		}
	  }

	  // Optional:  Delete all global objects allocated by libprotobuf.
	  google::protobuf::ShutdownProtobufLibrary();

	  return 0;
	}

#### cat pb_read.cc

	#include <iostream>
	#include <fstream>
	#include <string>
	#include "addressbook.pb.h"
	using namespace std;

	// Iterates though all people in the AddressBook and prints info about them.
	void ListPeople(const tutorial::AddressBook& address_book) {
	  for (int i = 0; i < address_book.person_size(); i++) {
		const tutorial::Person& person = address_book.person(i);

		cout << "Person ID: " << person.id() << endl;
		cout << "  Name: " << person.name() << endl;
		if (person.has_email()) {
		  cout << "  E-mail address: " << person.email() << endl;
		}

		for (int j = 0; j < person.phone_size(); j++) {
		  const tutorial::Person::PhoneNumber& phone_number = person.phone(j);

		  switch (phone_number.type()) {
			case tutorial::Person::MOBILE:
			  cout << "  Mobile phone #: ";
			  break;
			case tutorial::Person::HOME:
			  cout << "  Home phone #: ";
			  break;
			case tutorial::Person::WORK:
			  cout << "  Work phone #: ";
			  break;
		  }
		  cout << phone_number.number() << endl;
		}
	  }
	}

	// Main function:  Reads the entire address book from a file and prints all
	//   the information inside.
	int main(int argc, char* argv[]) {
	  // Verify that the version of the library that we linked against is
	  // compatible with the version of the headers we compiled against.
	  GOOGLE_PROTOBUF_VERIFY_VERSION;

	  if (argc != 2) {
		cerr << "Usage:  " << argv[0] << " ADDRESS_BOOK_FILE" << endl;
		return -1;
	  }

	  tutorial::AddressBook address_book;

	  {
		// Read the existing address book.
		fstream input(argv[1], ios::in | ios::binary);
		if (!address_book.ParseFromIstream(&input)) {
		  cerr << "Failed to parse address book." << endl;
		  return -1;
		}
	  }

	  ListPeople(address_book);

	  // Optional:  Delete all global objects allocated by libprotobuf.
	  google::protobuf::ShutdownProtobufLibrary();

	  return 0;
	}

#### cat Makefile  

	CC     = g++
	FLAGS  = -Wall
	PRO_F  = addressbook
	PRO_SRC= $(PRO_F).pb.cc
	LIBS   = -lprotobuf
	TARGET = pb_write pb_read

	all:$(TARGET) 

	pb_write: proto_file 
		$(CC) $(FLAGS) $(LIBS) -o $@ pb_write.cc $(PRO_SRC)

	pb_read: proto_file 
		$(CC) $(FLAGS) $(LIBS) -o $@ pb_read.cc $(PRO_SRC)

	proto_file: $(PRO_F).proto
		protoc -I=./ --cpp_out=./ $(PRO_F).proto

	.PHONEY:clean 
	clean:
		rm -f $(TARGET) $(OBJS) $(PRO_F).pb.h $(PRO_F).pb.cc

#### Files:

	[dennis@localhost google-code-sample]$ ls
	addressbook.proto  Makefile  pb_read.cc  pb_write.cc

#### Compile and run:

	[dennis@localhost google-code-sample]$ make
	protoc -I=./ --cpp_out=./ addressbook.proto
	g++ -Wall -lprotobuf -o pb_write pb_write.cc addressbook.pb.cc
	g++ -Wall -lprotobuf -o pb_read pb_read.cc addressbook.pb.cc
	[dennis@localhost google-code-sample]$ ls
	addressbook.pb.cc  addressbook.pb.h  addressbook.proto  Makefile  pb_read  pb_read.cc  pb_write  pb_write.cc
	[dennis@localhost google-code-sample]$ ./pb_write dennis
	dennis: File not found.  Creating a new file.
	Enter person ID number: 13
	Enter name: dennis
	Enter email address (blank for none): dennis.cpp@gmail.com
	Enter a phone number (or leave blank to finish): 14159265358
	Is this a mobile, home, or work phone? mobile
	Enter a phone number (or leave blank to finish): 
	[dennis@localhost google-code-sample]$ ./pb_write dennis
	Enter person ID number: 19
	Enter name: tina
	Enter email address (blank for none): tina.mm@gmail.com
	Enter a phone number (or leave blank to finish): 21714169628
	Is this a mobile, home, or work phone? home 
	Enter a phone number (or leave blank to finish): 
	[dennis@localhost google-code-sample]$ ./pb_read dennis
	Person ID: 13
	  Name: dennis
	  E-mail address: dennis.cpp@gmail.com
	  Mobile phone #: 14159265358
	Person ID: 19
	  Name: tina
	  E-mail address: tina.mm@gmail.com
	  Home phone #: 21714169628
	[dennis@localhost google-code-sample]$ ls
	addressbook.pb.cc  addressbook.pb.h  addressbook.proto  dennis  Makefile  pb_read  pb_read.cc  pb_write  pb_write.cc

#### Problem:

solution of error while loading shared libraries, 
look [here](../../../../2014/06/07/solution-of-error-while-loading-shared-libraries) 
for details.

#### Reference: 

* [Protocol Buffer Basics: C++](https://developers.google.com/protocol-buffers/docs/cpptutorial)
