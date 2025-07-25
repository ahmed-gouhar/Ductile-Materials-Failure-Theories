import numpy as np
import matplotlib.pyplot as plt
from numpy import sin,cos,pi

# inputs
SY = int(input('yield strength = ')) 
sx  = int(input('x-direction stress = '))
sy  = int(input('y-direction stress = '))
tau = int(input('shear stress = '))

# calculate principal stresses
s_avr = (sx+sy)/2
tau_max = ( ((sx-sy)/2)**2 + tau**2 )**0.5
s1 = s_avr + tau_max
s2 = s_avr - tau_max

# F.S.
FS1 = (SY/2)/tau_max # F.S. due to max. shear. theo.
S_von_mises = s1**2 - s1*s2 + s2**2
FS2 = ((SY**2)/(S_von_mises))**0.5


# Von-Mises elipse.
a = SY*2**0.5
b = SY*(2/3)**0.5
theta = np.linspace(0,2*pi,1000)
x = a*cos(theta)
y = b*sin(theta)
rot = pi/4
xd = cos(rot)*x - sin(rot)*y
yd = sin(rot)*x + cos(rot)*y

# Maximum-Shearing-Stress Criterion.
xp = [SY,SY,0,-SY,-SY,0,SY]
yp = [0,SY,SY,0,-SY,-SY,0]

# Figure
plt.figure(figsize=(10,7))
plt.plot(xd,yd,label='von mises theory')
plt.plot(xp,yp,label='maximum faliure theory',linestyle='--',linewidth=0.8,color='red')
plt.scatter(s1,s2)
plt.plot([0,0],[-SY - 50,SY + 50],linestyle='--',color='black')
plt.plot([-SY - 50,SY + 50],[0,0],linestyle='--',color='black')
plt.grid(True)
plt.legend(loc='upper left')
plt.show()


print('Maximum-Shearing-Stress Criterion Factor of Safety = {}'.format(FS1))
print('Von-Mises Criterion Factor of Safety = {}'.format(FS2))
