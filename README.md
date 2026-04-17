# Predator-prey-
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import odeint

# -------------------------------
# PARAMETERS (from Table 2)
# -------------------------------
r = 0.2  # Intrinsic growth rate of prey
a1 = 0.3  # Interaction rate (Prey decline)
a2 = 0.1  # Conversion rate (Predator birth)
b = 0.1  # Conversion of cannibalism to birth
alpha = 0.1  # Half-saturation constant
d = 0.1  # Natural death rate of predator
m = 0.1  # Intraspecific competition (Predator)

# Cannibalism values to compare (Simulation 3 style)
c_values = [0.1, 0.2, 0.3, 0.4, 0.5, 0.6]

# Time points
t = np.linspace(0, 200, 1000)
initial_conditions = [1.5, 1.5]


def model(z, t, c):
    N, P = z
    # Equation for Prey including Cannibalism (Holling Type II)
    dNdt = r * N - a1 * N * P + b * N - (c * N ** 2) / (alpha + N)
    # Equation for Predator including Intraspecific Competition
    dPdt = a2 * N * P - m * P ** 2 - d * P
    return [dNdt, dPdt]


# -------------------------------
# PLOTTING PREY AND PREDATOR RESPONSES
# -------------------------------
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 10), sharex=True)

for c in c_values:
    sol = odeint(model, initial_conditions, t, args=(c,))

    # Plot Prey Population (N)
    ax1.plot(t, sol[:, 0], label=f'c={c}')
    # Plot Predator Population (P)
    ax2.plot(t, sol[:, 1], label=f'c={c}')

# Formatting Prey Plot
ax1.set_title("Impact of Cannibalism Rate ($c$) on Prey Population ($N$)")
ax1.set_ylabel("Prey Population")
ax1.legend(loc='upper right', title="Cannibalism Rate")
ax1.grid(True, linestyle='--', alpha=0.7)

# Formatting Predator Plot
ax2.set_title("Impact of Cannibalism Rate ($c$) on Predator Population ($P$)")
ax2.set_xlabel("Time ($t$)")
ax2.set_ylabel("Predator Population")
ax2.legend(loc='upper right', title="Cannibalism Rate")
ax2.grid(True, linestyle='--', alpha=0.7)

plt.tight_layout()
plt.show()
