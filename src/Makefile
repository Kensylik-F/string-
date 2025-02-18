CC = gcc
CFLAGS = -g -Wall -Werror -Wextra -std=c11 -fprofile-arcs -ftest-coverage
LDFLAGS = -lcheck -lm

SOURCE = $(wildcard s21_*.c)

ifeq ($(shell uname -s), Linux)
	OPEN_CMD = xdg-open
endif
ifeq ($(shell uname -s), Darwin)
	OPEN_CMD = open
endif

all: clean s21_string.a test gcovr

object:
	@$(CC) $(CFLAGS) $(SOURCE) -c $<


s21_string.a: object
	@ar rcs s21_string.a *.o
	@rm -rf *.o 
	@ranlib s21_string.a
	
test: s21_string.a
	@$(CC) tests.c s21_string.a s21_string.c s21_sprintf.c -o test.out $(LDFLAGS)
	./test.out

gcovr: s21_string.a
	@mkdir report
	@ $(CC) -fprofile-arcs -ftest-coverage s21_string.a tests.c s21_string.c -o report/gcov_report $(LDFLAGS)
	@./report/gcov_report
	@gcovr --html-details --gcov-exclude tests.c -o ./report/report.html
	@$(OPEN_CMD) ./report/report.html

clean:
	@rm -rf *.o *.a *.out *.gcno *.gcda *.info test
	@rm -rf ./report/
