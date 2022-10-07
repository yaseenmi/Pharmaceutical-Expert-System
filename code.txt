from experta import *

grade = 0


# function for age
def age_answer(y):
    a = int(input(y))
    if a < 12:
        print("you are too young you must be at least 12 years old")
        return age_answer(y)

    if a > 120:
        print("invalid age")
        return age_answer(y)


# function for questions
def answer_grade(y):
    global grade
    x = int(input(y))
    if x <= 4 and x >= 1:
        grade += x
        return x
    else:
        return answer_grade(y)


# function for yes and no
def answer_yn(y):
    x = input(y)
    if x == 'yes' or x == 'y':
        return 'yes'
    elif x == 'no' or x == 'n':
        return 'no'
    return answer_yn(y)


class pharmacy(KnowledgeEngine):
    global grade

    @Rule(salience=10000)
    def start_beck(self):
        self.declare(Fact(age=age_answer(
            "enter your age \n")))

    @Rule(salience=9000)
    def pain1(self):
        self.declare(Fact(q1=answer_grade("choose a pain: \n1 رشح.\n2 صداع.\n3 التهاب حلق.\n")))

    # رشح بدون امراض مزمنة
    @Rule(Fact(q1=1))
    def ask_q_1(self):
        self.declare(Fact(p1=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(p1="no"), Fact(p1="n")))
    def ask_q_2(self):
        self.declare(Fact(p2=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(p2="no"), Fact(p2="n")))
    def med1(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر")

    # رشح مع امراض مزمنة
    @Rule(OR(Fact(p2="yes"), Fact(p2="y")))
    def chronic1(self):
        self.declare(Fact(q3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # رشح مع امراض مزمنة(ربو)
    @Rule(Fact(q3=2))
    def ask_q_3(self):
        self.declare(Fact(p11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p11="no"), Fact(p11="n")))
    def med2(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    @Rule(OR(Fact(p11="yes"), Fact(p11="y")))
    def chronic2(self):
        self.declare(Fact(q14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # رشح مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(q14=1))
    def ask_q_4(self):
        self.declare(Fact(p12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p12="no"), Fact(p12="n")))
    def med3(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(p12="yes"), Fact(p12="y")))
    def chronic3(self):
        self.declare(Fact(q15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(q15=3))
    def med4(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(q14=3))
    def ask_q_5(self):
        self.declare(Fact(p17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p17="no"), Fact(p17="n")))
    def med5(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(p17="yes"), Fact(p17="y")))
    def chronic4(self):
        self.declare(Fact(q22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(q22=1))
    def med6(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(ضغط)
    @Rule(Fact(q3=3))
    def ask_q_6(self):
        self.declare(Fact(p13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p13="no"), Fact(p13="n")))
    def med7(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين ")

    # رشح مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(p13="yes"), Fact(p13="y")))
    def chronic6(self):
        self.declare(Fact(q16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(q16=1))
    def ask_q_7(self):
        self.declare(Fact(p14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p14="no"), Fact(p14="n")))
    def med8(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(p14="yes"), Fact(p14="y")))
    def chronic7(self):
        self.declare(Fact(q17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(q17=2))
    def med9(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(q16=2))
    def ask_q_8(self):
        self.declare(Fact(p15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p15="no"), Fact(p15="n")))
    def med10(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(p15="yes"), Fact(p15="y")))
    def chronic9(self):
        self.declare(Fact(q19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(q19=1))
    def med11(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(سكري)
    @Rule(Fact(q3=1))
    def ask_q_9(self):
        self.declare(Fact(p3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p3="no"), Fact(p3="n")))
    def med12(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(p3="yes"), Fact(p3="y")))
    def chronic10(self):
        self.declare(Fact(q4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(q4=2))
    def ask_q_10(self):
        self.declare(Fact(p4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p4="no"), Fact(p4="n")))
    def med13(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(p4="yes"), Fact(p4="y")))
    def chronic11(self):
        self.declare(Fact(q5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(q5=3))
    def med14(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(q4=3))
    def ask_q_11(self):
        self.declare(Fact(p16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(p16="no"), Fact(p16="n")))
    def med15(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(p16="yes"), Fact(p16="y")))
    def chronic13(self):
        self.declare(Fact(q21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(q21=2))
    def med16(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع
    @Rule(OR(Fact(p1="yes"), Fact(p1="y")))
    def pain2(self):
        self.declare(Fact(q2=answer_grade("choose another pain: \n2 صداع.\n3 التهاب حلق.\n")))

    @Rule(Fact(q2=2))
    def ask_q_12(self):
        self.declare(Fact(p5=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(p5="no"), Fact(p5="n")))
    def ask_q_13(self):
        self.declare(Fact(p6=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(p6="no"), Fact(p6="n")))
    def med17(self):
        self.declare(Fact(med="true"))
        print("بانادول أصفر")

    # رشح + صداع مع امراض مزمنة
    @Rule(OR(Fact(p6="yes"), Fact(p6="y")))
    def chronic14(self):
        self.declare(Fact(pq3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # رشح + صداع مع امراض مزمنة(ربو)
    @Rule(Fact(pq3=2))
    def ask_q_14(self):
        self.declare(Fact(pp11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp11="no"), Fact(pp11="n")))
    def med18(self):
        self.declare(Fact(med="true"))
        print("بانادول أصفر")

    @Rule(OR(Fact(pp11="yes"), Fact(pp11="y")))
    def chronic15(self):
        self.declare(Fact(pq14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # رشح+ صداع مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(pq14=1))
    def ask_q_15(self):
        self.declare(Fact(pp12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp12="no"), Fact(pp12="n")))
    def med19(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح + صداع مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(pp12="yes"), Fact(pp12="y")))
    def chronic16(self):
        self.declare(Fact(pq15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(pq15=3))
    def med20(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(pq14=3))
    def ask_q_16(self):
        self.declare(Fact(pp17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp17="no"), Fact(pp17="n")))
    def med21(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(pp17="yes"), Fact(pp17="y")))
    def chronic17(self):
        self.declare(Fact(pq22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(pq22=1))
    def med22(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(ضغط)
    @Rule(Fact(pq3=3))
    def ask_q_17(self):
        self.declare(Fact(pp13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp13="no"), Fact(pp13="n")))
    def med23(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(pp13="yes"), Fact(pp13="y")))
    def chronic18(self):
        self.declare(Fact(pq16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(pq16=1))
    def ask_q_18(self):
        self.declare(Fact(pp14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp14="no"), Fact(pp14="n")))
    def med24(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح + صداع مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(pp14="yes"), Fact(pp14="y")))
    def chronic19(self):
        self.declare(Fact(pq17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(pq17=2))
    def med25(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(pq16=2))
    def ask_q_19(self):
        self.declare(Fact(pp15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp15="no"), Fact(pp15="n")))
    def med26(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(pp15="yes"), Fact(pp15="y")))
    def chronic20(self):
        self.declare(Fact(pq19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(pq19=1))
    def med27(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(سكري)
    @Rule(Fact(pq3=1))
    def ask_q_20(self):
        self.declare(Fact(pp3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp3="no"), Fact(pp3="n")))
    def med28(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح + صداع مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(pp3="yes"), Fact(pp3="y")))
    def chronic21(self):
        self.declare(Fact(pq4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(pq4=2))
    def ask_q_21(self):
        self.declare(Fact(pp4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp4="no"), Fact(pp4="n")))
    def med29(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح + صداع مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(pp4="yes"), Fact(pp4="y")))
    def chronic22(self):
        self.declare(Fact(pq5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(pq5=3))
    def med30(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(pq4=3))
    def ask_q_23(self):
        self.declare(Fact(pp16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp16="no"), Fact(pp16="n")))
    def med31(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # رشح + صداع مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(pp16="yes"), Fact(pp16="y")))
    def chronic23(self):
        self.declare(Fact(pq21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(pq21=2))
    def med32(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # رشح + صداع + التهاب حلق
    @Rule(OR(Fact(p5="yes"), Fact(p5="y")))
    def pain3(self):
        self.declare(Fact(ppq6=answer_grade("choose another pain:\n3 التهاب حلق.\n")))

    @Rule(Fact(ppq6=3))
    def ask_q_24(self):
        self.declare(Fact(ppp7=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(ppp7="no"), Fact(ppp7="n")))
    def med33(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر + أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة
    @Rule(OR(Fact(ppp7="yes"), Fact(ppp7="y")))
    def chronic24(self):
        self.declare(Fact(pq3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ربو)
    @Rule(Fact(ppq3=2))
    def ask_q_25(self):
        self.declare(Fact(ppp11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp11="no"), Fact(ppp11="n")))
    def med34(self):
        self.declare(Fact(med="true"))
        print("بنادول أصفر+أبيبول")

    @Rule(OR(Fact(ppp11="yes"), Fact(ppp11="y")))
    def chronic25(self):
        self.declare(Fact(ppq14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # رشح+ صداع + التهاب حلق مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(ppq14=1))
    def ask_q_26(self):
        self.declare(Fact(ppp12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp12="no"), Fact(ppp12="n")))
    def med35(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(ppp12="yes"), Fact(ppp12="y")))
    def chronic26(self):
        self.declare(Fact(ppq15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(ppq15=3))
    def med36(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(ppq14=3))
    def ask_q_27(self):
        self.declare(Fact(ppp17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp17="no"), Fact(pp17="n")))
    def med37(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(ppp17="yes"), Fact(ppp17="y")))
    def chronic27(self):
        self.declare(Fact(ppq22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(ppq22=1))
    def med38(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ضغط)
    @Rule(Fact(ppq3=3))
    def ask_q_28(self):
        self.declare(Fact(ppp13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp13="no"), Fact(ppp13="n")))
    def med39(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(ppp13="yes"), Fact(ppp13="y")))
    def chronic28(self):
        self.declare(Fact(ppq16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(ppq16=1))
    def ask_q_29(self):
        self.declare(Fact(ppp14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp14="no"), Fact(ppp14="n")))
    def med40(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(ppp14="yes"), Fact(ppp14="y")))
    def chronic29(self):
        self.declare(Fact(ppq17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(ppq17=2))
    def med41(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(ppq16=2))
    def ask_q_30(self):
        self.declare(Fact(ppp15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp15="no"), Fact(ppp15="n")))
    def med42(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(ppp15="yes"), Fact(ppp15="y")))
    def chronic30(self):
        self.declare(Fact(ppq19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(ppq19=1))
    def med43(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري)
    @Rule(Fact(ppq3=1))
    def ask_q_31(self):
        self.declare(Fact(ppp3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp3="no"), Fact(ppp3="n")))
    def med44(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(ppp3="yes"), Fact(ppp3="y")))
    def chronic31(self):
        self.declare(Fact(ppq4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(ppq4=2))
    def ask_q_32(self):
        self.declare(Fact(ppp4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp4="no"), Fact(ppp4="n")))
    def med45(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(ppp4="yes"), Fact(ppp4="y")))
    def chronic32(self):
        self.declare(Fact(ppq5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(ppq5=3))
    def med46(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(ppq4=3))
    def ask_q_33(self):
        self.declare(Fact(ppp16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(ppp16="no"), Fact(ppp16="n")))
    def med47(self):
        self.declare(Fact(med="true"))
        print("انادول أزرق + فيتامين سي+أبيبول")

    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(ppp16="yes"), Fact(ppp16="y")))
    def chronic33(self):
        self.declare(Fact(ppq21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(ppq21=2))
    def med48(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # رشح + التهاب حلق
    @Rule(Fact(q2=3))
    def ask_q_34(self):
        self.declare(Fact(p25=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(p25="no"), Fact(p25="n")))
    def ask_q_35(self):
        self.declare(Fact(p26=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(p26="no"), Fact(p26="n")))
    def med49(self):
        self.declare(Fact(med="true"))
        print("كلاريناز + أبيبول")

  # رشح + التهاب حلق مع امراض مزمنة
    @Rule(OR(Fact(p26="yes"), Fact(p26="y")))
    def chronic34(self):
        self.declare(Fact(pq32=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # رشح + التهاب حلق مع امراض مزمنة(ربو)
    @Rule(Fact(pq32=2))
    def ask_q_36(self):
        self.declare(Fact(pp112=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp112="no"), Fact(pp112="n")))
    def med50(self):
        self.declare(Fact(med="true"))
        print("كلاريناز + أبيبول")

    @Rule(OR(Fact(pp112="yes"), Fact(pp112="y")))
    def chronic35(self):
        self.declare(Fact(pq142=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # رشح+ التهاب حلق مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(pq142=1))
    def ask_q_37(self):
        self.declare(Fact(pp122=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp122="no"), Fact(pp122="n")))
    def med51(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(pp122="yes"), Fact(pp122="y")))
    def chronic36(self):
        self.declare(Fact(pq152=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(pq152=3))
    def med52(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(pq142=3))
    def ask_q_38(self):
        self.declare(Fact(pp172=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp172="no"), Fact(pp172="n")))
    def med53(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول ")

    # رشح + التهاب حلق مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(pp172="yes"), Fact(pp172="y")))
    def chronic37(self):
        self.declare(Fact(pq222=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(pq222=1))
    def med54(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ضغط)
    @Rule(Fact(pq32=3))
    def ask_q_39(self):
        self.declare(Fact(pp132=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp132="no"), Fact(pp132="n")))
    def med55(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(pp132="yes"), Fact(pp132="y")))
    def chronic38(self):
        self.declare(Fact(pq162=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(pq162=1))
    def ask_q_40(self):
        self.declare(Fact(pp142=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp142="no"), Fact(pp142="n")))
    def med56(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(pp142="yes"), Fact(pp142="y")))
    def chronic39(self):
        self.declare(Fact(pq172=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(pq172=2))
    def med57(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(pq162=2))
    def ask_q_41(self):
        self.declare(Fact(pp152=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp152="no"), Fact(pp152="n")))
    def med58(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(pp152="yes"), Fact(pp152="y")))
    def chronic40(self):
        self.declare(Fact(pq192=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(pq192=1))
    def med59(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(سكري)
    @Rule(Fact(pq32=1))
    def ask_q_42(self):
        self.declare(Fact(pp32=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp32="no"), Fact(pp32="n")))
    def med60(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(pp32="yes"), Fact(pp32="y")))
    def chronic41(self):
        self.declare(Fact(pq42=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(pq42=2))
    def ask_q_43(self):
        self.declare(Fact(pp42=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp42="no"), Fact(pp42="n")))
    def med61(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(pp42="yes"), Fact(pp42="y")))
    def chronic42(self):
        self.declare(Fact(pq52=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(pq52=3))
    def med62(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(pq42=3))
    def ask_q_44(self):
        self.declare(Fact(pp162=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(pp162="no"), Fact(pp162="n")))
    def med63(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح + التهاب حلق مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(pp162="yes"), Fact(pp162="y")))
    def chronic43(self):
        self.declare(Fact(pq212=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(pq212=2))
    def med64(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # رشح +التهاب حلق + صداع
    @Rule(OR(Fact(p25="yes"), Fact(p25="y")))
    def pain4(self):
        self.declare(Fact(ppq62=answer_grade("choose another pain:\n2 صداع.\n")))


    @Rule(Fact(ppq62=2))
    def ask_q_45(self):
        self.declare(Fact(ppp7=answer_yn("Do you have chronic diseases? (yes/ no)")))


    @Rule(OR(Fact(ppp72="no"), Fact(ppp72="n")))
    def med65(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة
    @Rule(OR(Fact(ppp72="yes"), Fact(ppp72="y")))
    def chronic44(self):
        self.declare(Fact(pq32=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ربو)
    @Rule(Fact(ppq32=2))
    def ask_q_46(self):
        self.declare(Fact(ppp112=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp112="no"), Fact(ppp112="n")))
    def med66(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    @Rule(OR(Fact(ppp112="yes"), Fact(ppp112="y")))
    def chronic45(self):
        self.declare(Fact(ppq142=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(ppq142=1))
    def ask_q_47(self):
        self.declare(Fact(ppp122=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp122="no"), Fact(ppp122="n")))
    def med67(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(ppp122="yes"), Fact(ppp122="y")))
    def chronic46(self):
        self.declare(Fact(ppq152=answer_grade("choose another chronic disease: \n3 ضغط\n")))


    @Rule(Fact(ppq152=3))
    def med68(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(ppq142=3))
    def ask_q_48(self):
        self.declare(Fact(ppp172=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp172="no"), Fact(pp172="n")))
    def med69(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(ppp172="yes"), Fact(ppp172="y")))
    def chronic47(self):
        self.declare(Fact(ppq222=answer_grade("choose another chronic disease: \n1 سكري\n")))


    @Rule(Fact(ppq222=1))
    def med70(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ضغط)
    @Rule(Fact(ppq32=3))
    def ask_q_49(self):
        self.declare(Fact(ppp132=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp132="no"), Fact(ppp132="n")))
    def med71(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(ppp132="yes"), Fact(ppp132="y")))
    def chronic48(self):
        self.declare(Fact(ppq162=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))


    @Rule(Fact(ppq162=1))
    def ask_q_50(self):
        self.declare(Fact(ppp142=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp142="no"), Fact(ppp142="n")))
    def med72(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(ppp142="yes"), Fact(ppp142="y")))
    def chronic49(self):
        self.declare(Fact(ppq172=answer_grade("choose another chronic disease:\n2 ربو\n")))


    @Rule(Fact(ppq172=2))
    def med73(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(ppq162=2))
    def ask_q_51(self):
        self.declare(Fact(ppp152=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp152="no"), Fact(ppp152="n")))
    def med74(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(ppp152="yes"), Fact(ppp152="y")))
    def chronic50(self):
        self.declare(Fact(ppq192=answer_grade("choose another chronic disease: \n1 سكري\n")))


    @Rule(Fact(ppq192=1))
    def med75(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح +التهاب حلق + صداع مع امراض مزمنة(سكري)
    @Rule(Fact(ppq32=1))
    def ask_q_52(self):
        self.declare(Fact(ppp32=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp32="no"), Fact(ppp32="n")))
    def med76(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(ppp32="yes"), Fact(ppp32="y")))
    def chronic51(self):
        self.declare(Fact(ppq42=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))


    @Rule(Fact(ppq42=2))
    def ask_q_53(self):
        self.declare(Fact(ppp42=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp42="no"), Fact(ppp42="n")))
    def med77(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(ppp42="yes"), Fact(ppp42="y")))
    def chronic52(self):
        self.declare(Fact(ppq52=answer_grade("choose another chronic disease:\n3 ضغط\n")))


    @Rule(Fact(ppq52=3))
    def med78(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(ppq42=3))
    def ask_q_54(self):
        self.declare(Fact(ppp162=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(ppp162="no"), Fact(ppp162="n")))
    def med79(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # رشح + صداع + التهاب حلق مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(ppp162="yes"), Fact(ppp162="y")))
    def chronic53(self):
        self.declare(Fact(ppq212=answer_grade("choose another chronic disease: \n2 ربو\n")))


    @Rule(Fact(ppq212=2))
    def med80(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

#-----------------------------------------------------------------------------------------------
    # صداع بدون امراض مزمنة
    @Rule(Fact(q1=2))
    def ask_qq_1(self):
        self.declare(Fact(qqp1=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(qqp1="no"), Fact(qqp1="n")))
    def medq16(self):
        print("بنادول أزرق")


    # صداع + رشح
    @Rule(OR(Fact(qqp1="yes"), Fact(qqp1="y")))
    def painq2(self):
        self.declare(Fact(qqq2=answer_grade("choose another pain: \n1 رشح.\n3 التهاب حلق.\n")))

    @Rule(Fact(qqq2=1))
    def ask_qq_12(self):
        self.declare(Fact(qqp5=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(qqp5="no"), Fact(qqp5="n")))
    def ask_qq_13(self):
        self.declare(Fact(qqp6=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(qqp6="no"), Fact(qqp6="n")))
    def medq17(self):
        self.declare(Fact(med="true"))
        print("بانادول أصفر")

    # صداع + رشح مع امراض مزمنة
    @Rule(OR(Fact(qqp6="yes"), Fact(qqp6="y")))
    def chronicq14(self):
        self.declare(Fact(qqpq3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # صداع + رشح مع امراض مزمنة(ربو)
    @Rule(Fact(qqpq3=2))
    def ask_qq_14(self):
        self.declare(Fact(qqpp11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp11="no"), Fact(qqpp11="n")))
    def medq18(self):
        self.declare(Fact(med="true"))
        print("بانادول أصفر")

    @Rule(OR(Fact(qqpp11="yes"), Fact(qqpp11="y")))
    def chronicq15(self):
        self.declare(Fact(qqpq14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # صداع + رشح مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(qqpq14=1))
    def ask_qq_15(self):
        self.declare(Fact(qqpp12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp12="no"), Fact(qqpp12="n")))
    def medq19(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # صداع + رشح مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(qqpp12="yes"), Fact(qqpp12="y")))
    def chronicq16(self):
        self.declare(Fact(qqpq15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(qqpq15=3))
    def medq20(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(qqpq14=3))
    def ask_qq_16(self):
        self.declare(Fact(qqpp17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp17="no"), Fact(qqpp17="n")))
    def medq21(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(qqpp17="yes"), Fact(qqpp17="y")))
    def chronicq17(self):
        self.declare(Fact(qqpq22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(qqpq22=1))
    def medq22(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(ضغط)
    @Rule(Fact(qqpq3=3))
    def ask_qq_17(self):
        self.declare(Fact(qqpp13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp13="no"), Fact(qqpp13="n")))
    def medq23(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(qqpp13="yes"), Fact(qqpp13="y")))
    def chronicq18(self):
        self.declare(Fact(qqpq16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(qqpq16=1))
    def ask_qq_18(self):
        self.declare(Fact(qqpp14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp14="no"), Fact(qqpp14="n")))
    def medq24(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # صداع + رشح مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(qqpp14="yes"), Fact(qqpp14="y")))
    def chronicq19(self):
        self.declare(Fact(qqpq17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(qqpq17=2))
    def medq25(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(qqpq16=2))
    def ask_qq_19(self):
        self.declare(Fact(qqpp15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp15="no"), Fact(qqpp15="n")))
    def medq26(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(qqpp15="yes"), Fact(qqpp15="y")))
    def chronicq20(self):
        self.declare(Fact(qqpq19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(qqpq19=1))
    def medq27(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(سكري)
    @Rule(Fact(qqpq3=1))
    def ask_qq_20(self):
        self.declare(Fact(qqpp3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp3="no"), Fact(qqpp3="n")))
    def medq28(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # صداع + رشح مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(qqpp3="yes"), Fact(qqpp3="y")))
    def chronicq21(self):
        self.declare(Fact(qqpq4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(qqpq4=2))
    def ask_qq_21(self):
        self.declare(Fact(qqpp4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp4="no"), Fact(qqpp4="n")))
    def medq29(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # صداع + رشح مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(qqpp4="yes"), Fact(qqpp4="y")))
    def chronicq22(self):
        self.declare(Fact(qqpq5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(qqpq5=3))
    def medq30(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(qqpq4=3))
    def ask_qq_23(self):
        self.declare(Fact(qqpp16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqpp16="no"), Fact(qqpp16="n")))
    def medq31(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي")

    # صداع + رشح مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(qqpp16="yes"), Fact(qqpp16="y")))
    def chronicq23(self):
        self.declare(Fact(qqpq21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(qqpq21=2))
    def medq32(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين")

    # صداع + رشح + التهاب حلق
    @Rule(OR(Fact(qqp5="yes"), Fact(qqp5="y")))
    def painq3(self):
        self.declare(Fact(qqppq6=answer_grade("choose another pain:\n3 التهاب حلق.\n")))

    @Rule(Fact(qqppq6=3))
    def ask_qq_24(self):
        self.declare(Fact(qqppp7=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(qqppp7="no"), Fact(qqppp7="n")))
    def medq33(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر + أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة
    @Rule(OR(Fact(qqppp7="yes"), Fact(qqppp7="y")))
    def chronicq24(self):
        self.declare(Fact(qqpq3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ربو)
    @Rule(Fact(qqppq3=2))
    def ask_qq_25(self):
        self.declare(Fact(qqppp11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp11="no"), Fact(qqppp11="n")))
    def medq34(self):
        self.declare(Fact(med="true"))
        print("بنادول أصفر+أبيبول")

    @Rule(OR(Fact(qqppp11="yes"), Fact(ppp11="y")))
    def chronicq25(self):
        self.declare(Fact(qqppq14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(qqppq14=1))
    def ask_qq_26(self):
        self.declare(Fact(qqppp12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp12="no"), Fact(qqppp12="n")))
    def medq35(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(qqppp12="yes"), Fact(qqppp12="y")))
    def chronicq26(self):
        self.declare(Fact(qqppq15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(qqppq15=3))
    def medq36(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(qqppq14=3))
    def ask_qq_27(self):
        self.declare(Fact(qqppp17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp17="no"), Fact(qqpp17="n")))
    def medq37(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(qqppp17="yes"), Fact(qqppp17="y")))
    def chronicq27(self):
        self.declare(Fact(qqppq22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(qqppq22=1))
    def medq38(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ضغط)
    @Rule(Fact(qqppq3=3))
    def ask_qq_28(self):
        self.declare(Fact(qqppp13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp13="no"), Fact(qqppp13="n")))
    def medq39(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(qqppp13="yes"), Fact(qqppp13="y")))
    def chronicq28(self):
        self.declare(Fact(qqppq16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(qqppq16=1))
    def ask_qq_29(self):
        self.declare(Fact(qqppp14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp14="no"), Fact(qqppp14="n")))
    def medq40(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(qqppp14="yes"), Fact(qqppp14="y")))
    def chronicq29(self):
        self.declare(Fact(qqppq17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(qqppq17=2))
    def medq41(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(qqppq16=2))
    def ask_qq_30(self):
        self.declare(Fact(qqppp15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp15="no"), Fact(qqppp15="n")))
    def medq42(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(qqppp15="yes"), Fact(qqppp15="y")))
    def chronicq30(self):
        self.declare(Fact(qqppq19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(qqppq19=1))
    def medq43(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(سكري)
    @Rule(Fact(qqppq3=1))
    def ask_qq_31(self):
        self.declare(Fact(qqppp3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp3="no"), Fact(qqppp3="n")))
    def medq44(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(qqppp3="yes"), Fact(qqppp3="y")))
    def chronicq31(self):
        self.declare(Fact(qqppq4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(qqppq4=2))
    def ask_qq_32(self):
        self.declare(Fact(qqppp4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp4="no"), Fact(qqppp4="n")))
    def medq45(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(qqppp4="yes"), Fact(qqppp4="y")))
    def chronicq32(self):
        self.declare(Fact(qqppq5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(qqppq5=3))
    def medq46(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(qqppq4=3))
    def ask_qq_33(self):
        self.declare(Fact(qqppp16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(qqppp16="no"), Fact(qqppp16="n")))
    def medq47(self):
        self.declare(Fact(med="true"))
        print("انادول أزرق + فيتامين سي+أبيبول")

    # صداع + رشح + التهاب حلق مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(qqppp16="yes"), Fact(qqppp16="y")))
    def chronicq33(self):
        self.declare(Fact(qqppq21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(qqppq21=2))
    def medq48(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # صداع + التهاب حلق
    @Rule(Fact(qqq2=3))
    def ask_qq_34(self):
        self.declare(Fact(qqp25=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(qqp25="no"), Fact(qqp25="n")))
    def ask_qq_35(self):
        print("بنادول أزرق + أبيبول")


    # صداع +التهاب حلق + رشح
    @Rule(OR(Fact(qqp25="yes"), Fact(qqp25="y")))
    def painq4(self):
        self.declare(Fact(qqppq62=answer_grade("choose another pain:\n1 رشح.\n")))


    @Rule(Fact(qqppq62=1))
    def ask_qq_45(self):
        self.declare(Fact(qqppp72=answer_yn("Do you have chronic diseases? (yes/ no)")))


    @Rule(OR(Fact(qqppp72="no"), Fact(qqppp72="n")))
    def medq65(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة
    @Rule(OR(Fact(qqppp72="yes"), Fact(qqppp72="y")))
    def chronicq44(self):
        self.declare(Fact(qqppq32=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ربو)
    @Rule(Fact(qqppq32=2))
    def ask_qq_46(self):
        self.declare(Fact(qqppp112=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp112="no"), Fact(qqppp112="n")))
    def medq66(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    @Rule(OR(Fact(qqppp112="yes"), Fact(qqppp112="y")))
    def chronicq45(self):
        self.declare(Fact(qqppq142=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(qqppq142=1))
    def ask_qq_47(self):
        self.declare(Fact(qqppp122=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp122="no"), Fact(qqppp122="n")))
    def medq67(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(qqppp122="yes"), Fact(qqppp122="y")))
    def chronicq46(self):
        self.declare(Fact(qqppq152=answer_grade("choose another chronic disease: \n3 ضغط\n")))


    @Rule(Fact(qqppq152=3))
    def medq68(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(qqppq142=3))
    def ask_qq_48(self):
        self.declare(Fact(qqppp172=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp172="no"), Fact(qqpp172="n")))
    def medq69(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(qqppp172="yes"), Fact(qqppp172="y")))
    def chronicq47(self):
        self.declare(Fact(qqppq222=answer_grade("choose another chronic disease: \n1 سكري\n")))


    @Rule(Fact(qqppq222=1))
    def medq70(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ضغط)
    @Rule(Fact(qqppq32=3))
    def ask_qq_49(self):
        self.declare(Fact(qqppp132=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp132="no"), Fact(qqppp132="n")))
    def medq71(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(qqppp132="yes"), Fact(qqppp132="y")))
    def chronicq48(self):
        self.declare(Fact(qqppq162=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))


    @Rule(Fact(qqppq162=1))
    def ask_qq_50(self):
        self.declare(Fact(qqppp142=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp142="no"), Fact(qqppp142="n")))
    def medq72(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(qqppp142="yes"), Fact(qqppp142="y")))
    def chronicq49(self):
        self.declare(Fact(qqppq172=answer_grade("choose another chronic disease:\n2 ربو\n")))


    @Rule(Fact(qqppq172=2))
    def medq73(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(qqppq162=2))
    def ask_qq_51(self):
        self.declare(Fact(qqppp152=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp152="no"), Fact(qqppp152="n")))
    def medq74(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(qqppp152="yes"), Fact(qqppp152="y")))
    def chronicq50(self):
        self.declare(Fact(qqppq192=answer_grade("choose another chronic disease: \n1 سكري\n")))


    @Rule(Fact(qqppq192=1))
    def medq75(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(سكري)
    @Rule(Fact(qqppq32=1))
    def ask_qq_52(self):
        self.declare(Fact(qqppp32=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp32="no"), Fact(qqppp32="n")))
    def medq76(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(qqppp32="yes"), Fact(qqppp32="y")))
    def chronicq51(self):
        self.declare(Fact(qqppq42=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))


    @Rule(Fact(qqppq42=2))
    def ask_qq_53(self):
        self.declare(Fact(qqppp42=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp42="no"), Fact(qqppp42="n")))
    def medq77(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(qqppp42="yes"), Fact(qqppp42="y")))
    def chronicq52(self):
        self.declare(Fact(qqppq52=answer_grade("choose another chronic disease:\n3 ضغط\n")))


    @Rule(Fact(qqppq52=3))
    def medq78(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(qqppq42=3))
    def ask_qq_54(self):
        self.declare(Fact(qqppp162=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(qqppp162="no"), Fact(qqppp162="n")))
    def medq79(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # صداع +التهاب حلق + رشح مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(qqppp162="yes"), Fact(qqppp162="y")))
    def chronicq53(self):
        self.declare(Fact(qqppq212=answer_grade("choose another chronic disease: \n2 ربو\n")))


    @Rule(Fact(qqppq212=2))
    def medq80(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

#------------------------------------------------------------------------------------------
 # التهاب حلق بدون امراض مزمنة
    @Rule(Fact(q1=3))
    def ask_qqq_1(self):
        self.declare(Fact(eqqp1=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(eqqp1="no"), Fact(eqqp1="n")))
    def ask_qqq_2(self):
        print("أبيبول")

    # التهاب حلق + رشح
    @Rule(OR(Fact(eqqp1="yes"), Fact(eqqp1="y")))
    def painqq2(self):
        self.declare(Fact(eqqq2=answer_grade("choose another pain: \n1 رشح.\n2 صداع.\n")))

    @Rule(Fact(eqqq2=1))
    def ask_qqq_12(self):
        self.declare(Fact(eqqp5=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(eqqp5="no"), Fact(eqqp5="n")))
    def ask_qqq_13(self):
        self.declare(Fact(eqqp6=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(eqqp6="no"), Fact(eqqp6="n")))
    def medqq17(self):
        self.declare(Fact(med="true"))
        print("كلاريناز + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة
    @Rule(OR(Fact(eqqp6="yes"), Fact(eqqp6="y")))
    def chronicqq14(self):
        self.declare(Fact(eqqpq3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # التهاب حلق + رشح مع امراض مزمنة(ربو)
    @Rule(Fact(eqqpq3=2))
    def ask_qqq_14(self):
        self.declare(Fact(eqqpp11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp11="no"), Fact(eqqpp11="n")))
    def medqq18(self):
        self.declare(Fact(med="true"))
        print("كلاريناز + أبيبول")

    @Rule(OR(Fact(eqqpp11="yes"), Fact(eqqpp11="y")))
    def chronicqq15(self):
        self.declare(Fact(eqqpq14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # التهاب حلق + رشح مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(eqqpq14=1))
    def ask_qqq_15(self):
        self.declare(Fact(eqqpp12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp12="no"), Fact(eqqpp12="n")))
    def medqq19(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(eqqpp12="yes"), Fact(eqqpp12="y")))
    def chronicqq16(self):
        self.declare(Fact(eqqpq15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(eqqpq15=3))
    def medqq20(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(eqqpq14=3))
    def ask_qqq_16(self):
        self.declare(Fact(eqqpp17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp17="no"), Fact(eqqpp17="n")))
    def medqq21(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(eqqpp17="yes"), Fact(eqqpp17="y")))
    def chronicqq17(self):
        self.declare(Fact(eqqpq22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(eqqpq22=1))
    def medqq22(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ضغط)
    @Rule(Fact(eqqpq3=3))
    def ask_qqq_17(self):
        self.declare(Fact(eqqpp13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp13="no"), Fact(eqqpp13="n")))
    def medqq23(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(eqqpp13="yes"), Fact(eqqpp13="y")))
    def chronicqq18(self):
        self.declare(Fact(eqqpq16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(eqqpq16=1))
    def ask_qqq_18(self):
        self.declare(Fact(eqqpp14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp14="no"), Fact(eqqpp14="n")))
    def medqq24(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(eqqpp14="yes"), Fact(eqqpp14="y")))
    def chronicqq19(self):
        self.declare(Fact(eqqpq17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(eqqpq17=2))
    def medqq25(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(eqqpq16=2))
    def ask_qqq_19(self):
        self.declare(Fact(eqqpp15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp15="no"), Fact(eqqpp15="n")))
    def medqq26(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(eqqpp15="yes"), Fact(eqqpp15="y")))
    def chronicqq20(self):
        self.declare(Fact(eqqpq19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(eqqpq19=1))
    def medqq27(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(سكري)
    @Rule(Fact(eqqpq3=1))
    def ask_qqq_20(self):
        self.declare(Fact(eqqpp3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp3="no"), Fact(eqqpp3="n")))
    def medqq28(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(eqqpp3="yes"), Fact(eqqpp3="y")))
    def chronicqq21(self):
        self.declare(Fact(eqqpq4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(eqqpq4=2))
    def ask_qqq_21(self):
        self.declare(Fact(eqqpp4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp4="no"), Fact(eqqpp4="n")))
    def medqq29(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(eqqpp4="yes"), Fact(eqqpp4="y")))
    def chronicqq22(self):
        self.declare(Fact(eqqpq5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(eqqpq5=3))
    def medqq30(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(eqqpq4=3))
    def ask_qqq_23(self):
        self.declare(Fact(eqqpp16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqpp16="no"), Fact(eqqpp16="n")))
    def medqq31(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")

    # التهاب حلق + رشح مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(eqqpp16="yes"), Fact(eqqpp16="y")))
    def chronicqq23(self):
        self.declare(Fact(eqqpq21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(eqqpq21=2))
    def medqq32(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

    # التهاب حلق + رشح + صداع
    @Rule(OR(Fact(eqqp5="yes"), Fact(eqqp5="y")))
    def painqq3(self):
        self.declare(Fact(eqqppq6=answer_grade("choose another pain:\n2 صداع.\n")))

    @Rule(Fact(eqqppq6=2))
    def ask_qqq_24(self):
        self.declare(Fact(eqqppp7=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(eqqppp7="no"), Fact(eqqppp7="n")))
    def medqq33(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر + أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة
    @Rule(OR(Fact(eqqppp7="yes"), Fact(eqqppp7="y")))
    def chronicqq24(self):
        self.declare(Fact(eqqpq3=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ربو)
    @Rule(Fact(eqqppq3=2))
    def ask_qqq_25(self):
        self.declare(Fact(eqqppp11=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp11="no"), Fact(eqqppp11="n")))
    def medqq34(self):
        self.declare(Fact(med="true"))
        print("بنادول أصفر+أبيبول")

    @Rule(OR(Fact(eqqppp11="yes"), Fact(eppp11="y")))
    def chronicqq25(self):
        self.declare(Fact(eqqppq14=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(eqqppq14=1))
    def ask_qqq_26(self):
        self.declare(Fact(eqqppp12=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp12="no"), Fact(eqqppp12="n")))
    def medqq35(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(eqqppp12="yes"), Fact(eqqppp12="y")))
    def chronicqq26(self):
        self.declare(Fact(eqqppq15=answer_grade("choose another chronic disease: \n3 ضغط\n")))

    @Rule(Fact(eqqppq15=3))
    def medqq36(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(eqqppq14=3))
    def ask_qqq_27(self):
        self.declare(Fact(eqqppp17=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp17="no"), Fact(eqqpp17="n")))
    def medqq37(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(eqqppp17="yes"), Fact(eqqppp17="y")))
    def chronicqq27(self):
        self.declare(Fact(eqqppq22=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(eqqppq22=1))
    def medqq38(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ضغط)
    @Rule(Fact(eqqppq3=3))
    def ask_qqq_28(self):
        self.declare(Fact(eqqppp13=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp13="no"), Fact(eqqppp13="n")))
    def medqq39(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(eqqppp13="yes"), Fact(eqqppp13="y")))
    def chronicqq28(self):
        self.declare(Fact(eqqppq16=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))

    @Rule(Fact(eqqppq16=1))
    def ask_qqq_29(self):
        self.declare(Fact(eqqppp14=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp14="no"), Fact(eqqppp14="n")))
    def medqq40(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(eqqppp14="yes"), Fact(eqqppp14="y")))
    def chronicqq29(self):
        self.declare(Fact(eqqppq17=answer_grade("choose another chronic disease:\n2 ربو\n")))

    @Rule(Fact(eqqppq17=2))
    def medqq41(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(eqqppq16=2))
    def ask_qqq_30(self):
        self.declare(Fact(eqqppp15=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp15="no"), Fact(eqqppp15="n")))
    def medqq42(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(eqqppp15="yes"), Fact(eqqppp15="y")))
    def chronicqq30(self):
        self.declare(Fact(eqqppq19=answer_grade("choose another chronic disease: \n1 سكري\n")))

    @Rule(Fact(eqqppq19=1))
    def medqq43(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(سكري)
    @Rule(Fact(eqqppq3=1))
    def ask_qqq_31(self):
        self.declare(Fact(eqqppp3=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp3="no"), Fact(eqqppp3="n")))
    def medqq44(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(eqqppp3="yes"), Fact(eqqppp3="y")))
    def chronicqq31(self):
        self.declare(Fact(eqqppq4=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))

    @Rule(Fact(eqqppq4=2))
    def ask_qqq_32(self):
        self.declare(Fact(eqqppp4=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp4="no"), Fact(eqqppp4="n")))
    def medqq45(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(eqqppp4="yes"), Fact(eqqppp4="y")))
    def chronicqq32(self):
        self.declare(Fact(eqqppq5=answer_grade("choose another chronic disease:\n3 ضغط\n")))

    @Rule(Fact(eqqppq5=3))
    def medqq46(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين +أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(eqqppq4=3))
    def ask_qqq_33(self):
        self.declare(Fact(eqqppp16=answer_yn("Do you have another chronic disease? (yes / no)")))

    @Rule(OR(Fact(eqqppp16="no"), Fact(eqqppp16="n")))
    def medqq47(self):
        self.declare(Fact(med="true"))
        print("انادول أزرق + فيتامين سي+أبيبول")

    # التهاب حلق + رشح + صداع مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(eqqppp16="yes"), Fact(eqqppp16="y")))
    def chronicqq33(self):
        self.declare(Fact(eqqppq21=answer_grade("choose another chronic disease: \n2 ربو\n")))

    @Rule(Fact(eqqppq21=2))
    def medqq48(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين+أبيبول")

    # التهاب حلق + صداع
    @Rule(Fact(eqqq2=2))
    def ask_qqq_34(self):
        self.declare(Fact(eqqp25=answer_yn("Do you have another pain? (yes / no)")))

    @Rule(OR(Fact(eqqp25="no"), Fact(eqqp25="n")))
    def ask_qqq_35(self):
        print("بانادول أزرق + أبيبول")

    # التهاب حلق + صداع + رشح
    @Rule(OR(Fact(eqqp25="yes"), Fact(eqqp25="y")))
    def painqq4(self):
        self.declare(Fact(eqqppq62=answer_grade("choose another pain:\n1 رشح.\n")))

    @Rule(Fact(eqqppq62=1))
    def ask_qqq_45(self):
        self.declare(Fact(eqqppp72=answer_yn("Do you have chronic diseases? (yes/ no)")))

    @Rule(OR(Fact(eqqppp72="no"), Fact(eqqppp72="n")))
    def medqq65(self):
        self.declare(Fact(med="true"))
        print("بانادول اصفر + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة
    @Rule(OR(Fact(eqqppp72="yes"), Fact(eqqppp72="y")))
    def chronicqq44(self):
        self.declare(Fact(eqqppq32=answer_grade("choose a chronic disease: \n1 سكري\n2 ربو\n3 ضغط\n")))


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ربو)
    @Rule(Fact(eqqppq32=2))
    def ask_qqq_46(self):
        self.declare(Fact(eqqppp112=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp112="no"), Fact(eqqppp112="n")))
    def medqq66(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    @Rule(OR(Fact(eqqppp112="yes"), Fact(eqqppp112="y")))
    def chronicqq45(self):
        self.declare(Fact(eqqppq142=answer_grade("choose another chronic disease: \n1 سكري\n3 ضغط\n")))


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ربو,سكري)
    @Rule(Fact(eqqppq142=1))
    def ask_qqq_47(self):
        self.declare(Fact(eqqppp122=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp122="no"), Fact(eqqppp122="n")))
    def medqq67(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ربو,سكري,ضغط)
    @Rule(OR(Fact(eqqppp122="yes"), Fact(eqqppp122="y")))
    def chronicqq46(self):
        self.declare(Fact(eqqppq152=answer_grade("choose another chronic disease: \n3 ضغط\n")))


    @Rule(Fact(eqqppq152=3))
    def medqq68(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ربو,ضغط)
    @Rule(Fact(eqqppq142=3))
    def ask_qqq_48(self):
        self.declare(Fact(eqqppp172=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp172="no"), Fact(eqqpp172="n")))
    def medqq69(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ربو,ضغط,سكري)
    @Rule(OR(Fact(eqqppp172="yes"), Fact(eqqppp172="y")))
    def chronicqq47(self):
        self.declare(Fact(eqqppq222=answer_grade("choose another chronic disease: \n1 سكري\n")))


    @Rule(Fact(eqqppq222=1))
    def medqq70(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ضغط)
    @Rule(Fact(eqqppq32=3))
    def ask_qqq_49(self):
        self.declare(Fact(eqqppp132=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp132="no"), Fact(eqqppp132="n")))
    def medqq71(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ضغط,سكري)
    @Rule(OR(Fact(eqqppp132="yes"), Fact(eqqppp132="y")))
    def chronicqq48(self):
        self.declare(Fact(eqqppq162=answer_grade("choose another chronic disease: \n1 سكري\n2 ربو\n")))


    @Rule(Fact(eqqppq162=1))
    def ask_qqq_50(self):
        self.declare(Fact(eqqppp142=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp142="no"), Fact(eqqppp142="n")))
    def medqq72(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ضغط,سكري,ربو)
    @Rule(OR(Fact(eqqppp142="yes"), Fact(eqqppp142="y")))
    def chronicqq49(self):
        self.declare(Fact(eqqppq172=answer_grade("choose another chronic disease:\n2 ربو\n")))


    @Rule(Fact(eqqppq172=2))
    def medqq73(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع+ رشح مع امراض مزمنة(ضغط,ربو)
    @Rule(Fact(eqqppq162=2))
    def ask_qqq_51(self):
        self.declare(Fact(eqqppp152=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp152="no"), Fact(eqqppp152="n")))
    def medqq74(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(ضغط,ربو,سكري)
    @Rule(OR(Fact(eqqppp152="yes"), Fact(eqqppp152="y")))
    def chronicqq50(self):
        self.declare(Fact(eqqppq192=answer_grade("choose another chronic disease: \n1 سكري\n")))


    @Rule(Fact(eqqppq192=1))
    def medqq75(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(سكري)
    @Rule(Fact(eqqppq32=1))
    def ask_qqq_52(self):
        self.declare(Fact(eqqppp32=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp32="no"), Fact(eqqppp32="n")))
    def medqq76(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(سكري,ربو)
    @Rule(OR(Fact(eqqppp32="yes"), Fact(eqqppp32="y")))
    def chronicqq51(self):
        self.declare(Fact(eqqppq42=answer_grade("choose another chronic disease: \n2 ربو\n3 ضغط\n")))


    @Rule(Fact(eqqppq42=2))
    def ask_qqq_53(self):
        self.declare(Fact(eqqppp42=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp42="no"), Fact(eqqppp42="n")))
    def medqq77(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # التهاب حلق + صداع+ رشح مع امراض مزمنة(سكري,ربو,ضغط)
    @Rule(OR(Fact(eqqppp42="yes"), Fact(eqqppp42="y")))
    def chronicqq52(self):
        self.declare(Fact(qqppq52=answer_grade("choose another chronic disease:\n3 ضغط\n")))


    @Rule(Fact(eqqppq52=3))
    def medqq78(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(سكري,ضغط)
    @Rule(Fact(eqqppq42=3))
    def ask_qqq_54(self):
        self.declare(Fact(eqqppp162=answer_yn("Do you have another chronic disease? (yes / no)")))


    @Rule(OR(Fact(eqqppp162="no"), Fact(eqqppp162="n")))
    def medqq79(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + فيتامين سي + أبيبول")


    # التهاب حلق + صداع + رشح مع امراض مزمنة(سكري,ضغط,ربو)
    @Rule(OR(Fact(eqqppp162="yes"), Fact(eqqppp162="y")))
    def chronicqq53(self):
        self.declare(Fact(eqqppq212=answer_grade("choose another chronic disease: \n2 ربو\n")))


    @Rule(Fact(eqqppq212=2))
    def medqq80(self):
        self.declare(Fact(med="true"))
        print("بانادول أزرق + قطرة اوتريفين + أبيبول")

engine = pharmacy()
engine.reset()  # Prepare the engine for the execution.
engine.run()  # Run it!
