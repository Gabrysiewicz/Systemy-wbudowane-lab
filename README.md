# Systemy-wbudowane-lab
Praca po zajęciach
```
#define F_CPU 1000000L
#include <avr/io.h>
#include <avr/interrupt.h>


ISR(TIMER0_OVF_vect){
	PORTA = ~PORTA;
	TCNT0 = 0;
}

int main(void)
{
	// Konfiguracja PORTU A
	DDRA = 255;
	PORTA = 255;

	// Konfiguracja Timer0 tryb normal, prescaler (s.107-110)
	TCCR0 |= (0 << WGM01) | (0 << WGM00 );				 //Timer0 8-bit tryb normal
	TCCR0 |= (1 << CS02 ) | (0 << CS01  ) | (1<< CS00 ); //Timer0 8-bit Prescaler clk/1024
	
	// Wartość początkowa dla TCNT0
	TCNT0 = 0;
	// Globalne zezwolenie na przerwania
	sei();

	// Zezwolenie na przerwanie dla wybranego trybu pracy timera (s.112)
	TIMSK |= (1 << TOIE0);
	TIFR |= (1 << TOV0); // 00000100
	int t = 0;
	while (1)
	{
		if (TCNT0 >= 100)
		{
			TCNT0 = 0;          // reset counter
			t++;
		}
		if(t >= 10){
			PORTA = ~PORTA;    // LED swap
			t=0;
		}
	}
}

/*
#include <avr/io.h> 
#include <util/delay.h> 
#define F_CPU 1000000L 
void odlicz(int il_sekund) { 
	TCCR0|=(1<<CS00)|(1<<CS02); //ustawienie preskalera 
	int licz=0; TCNT0=155; //ustawienie wartości początkowej, licznik odmierza do 255 
	while(licz<=il_sekund*10){ 
		if(TIFR&=(1<<TOV0)){  //sprawdzenie czy ustawiona została flaga przepełnienia 
			TIFR|=(1<<TOV0);//zerowanie flagi przepełnienia 
			TCNT0=155;// ponowne ustawienie wartości początkowej 
			licz++; 
		} 
	}  
	PORTA = ~PORTA; 
	TCCR0 = 0x00; 
} 
	int main(void) { 
		DDRA = 0b11111111; 
		PORTA= 0b11111111;
		while(1) { 
			odlicz(1); //wywołanie procedury, w tym przypadku odmierza 5 sekund 
		} 
	return 0; 
}
*/
```
