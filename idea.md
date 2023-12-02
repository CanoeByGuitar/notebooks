```cpp
// Init x_p, v_p, m_p, F_p, C_p, dt

// P2G
  // mass p2g
  for(p = 1 ... particle_num){// loop over all particles
    for(i = p1 ... pn){ 			// loop over mapped grid nodes for current particle
      m_i += S_ip * m_p;  		// S_ip = S(xp - xi), S(*) is cubic kernel
    }
  }
  // velocity p2g
  for(p = 1 ... particle_num){	// loop over all particles
    for(i = p1 ... pn){ 				// loop over mapped grid nodes for current particle
      v_i += S_ip * m_p * v_p;	// temp for momentum
    }
  }
  for(i = 1 ... node_num){// loop over all particles
    v_i = v_i / m_i;
  }

// COMPUTE FORCE
  for(i = 1 ... node_num){// loop over all particles
    f_i = (0, -9.8, 0); // f_ext
    f_i += COMPUTE_STRESS(F_p); // f_int
  }

// UPDATE VELOCOITY
	// traditional mpm
	std::vector<vec3> v_i_temp(node_num);
	for(p = 1 ... node_num){ // loop over all grid nodes
    v_i_temp[i] = v_i + (f_i / m_i) * dt;
  }



	// seperate to contact pair
	for(p = 1 ... particle_num){ // loop over all particles
    for(i = p1 ... pn){
      if(grad_cp.norm2() > grad_ci.norm2() ){
        	grad_ci = grad_cp;
      }
    }
  }
	std::vector<float> m1(node_num), m2(node_num);
	std::vector<vec3>  v1(node_num), v2(node_num);
	for (p = 1 ... particle_num){
    for(i = p1 ... pn){
      if(grad_cp * grad_ci >= 0){
        m1[i]	+= S_ip * m_p;
        v1[i] += S_ip * v_p; // ? need to be updated by mpm force?
      }else{
        m2[i] += S_ip * m_p;
        v2[i] += S_ip * v_p; // ? need to be updated by mpm force?
      }
    }
  }
	// compute contact force and update v_i_temp to v_i
	std::vector<float> m_cm(node_num);
	for(i = 1 ... node_num){
    if(m1[i] > 0 && m2[i] > 0){
      m_cm[i] = m1[i] + m2[i];
      v_cm[i] = (v1[i] * m1[i] + v2[i] * m2[i]) / (m1[i] + m2[i]);
      [n1, n2] = COMPUTE_NORMAL(m_p); // n1 = -n2
      
      [s_A, s_B] = COMPUTE_OTRH_BASIS(n1); // compute tangential direction
      // contact pair 1
      {
        if((v_cm[i] - v1[i]) * n1 > 0){ 
          f_nor = m1[i] * (v_cm[i] - v1[i]) / dt * n1;
          f_tan_A = m1[i] * (v_cm[i] - v1[i]) / dt * s_A;
          f_tan_B = m1[i] * (v_cm[i] - v1[i]) / dt * s_B;
          f_tan   = (f_tan_A * s_A + f_tan_B * s_B).norm();
          s				= (f_tan_A * s_A + f_tan_B * s_B).normalized();
          f_ct_1[i]  = f_nor * n1 + min(mu * abs(f_nor), abs(f_tan)) * f_tan * s;
        }
      }
      
      // contact pair 2
      {
        if((v_cm[i] - v1[i]) * n2 > 0){ 
          f_nor = m1[i] * (v_cm[i] - v1[i]) / dt * n2;
          f_tan_A = m1[i] * (v_cm[i] - v1[i]) / dt * s_A;
          f_tan_B = m1[i] * (v_cm[i] - v1[i]) / dt * s_B;
          f_tan   = (f_tan_A * s_A + f_tan_B * s_B).norm();
          s				= (f_tan_A * s_A + f_tan_B * s_B).normalized();
          f_ct_2[i]  = f_nor * n2 + min(mu * abs(f_nor), abs(f_tan)) * f_tan * s;
        }
      }
      
     	v_1[i] += f_ct_1[i] / m1[i] * dt;
      v_2[i] += f_ct_2[i] / m2[i] * dt;
      v[i] = (v1[i] * m1[i] + v2[i] * m2[i]) / (m1[i] + m2[i]);
    }
  }

// Update F_p
F_p += (grad_vp * F_p) * dt;

// G2P
  for(p = 1 ... particle_num){	// loop over all particles
    for(i = p1 ... pn){ 				// loop over mapped grid nodes for current particle
      v_p += S_ip * m_i * v_i;	// temp for momentum
    }
    v_p = v_p / m_p;
  }

// Advection
x_p = GET_ADVECTION(transfer_scheme); // APIC/FLIP/PIC

```





``` cpp
	// seperate to contact pair
	for(p = 1 ... particle_num){ // loop over all particles
    for(i = p1 ... pn){
      if(grad_cp.norm2() > grad_ci.norm2() ){
        	grad_ci = grad_cp;
      }
    }
  }
	std::vector<float> m1(node_num), m2(node_num);
	std::vector<vec3>  v1(node_num), v2(node_num);
	for (p = 1 ... particle_num){
    for(i = p1 ... pn){
      if(grad_cp * grad_ci >= 0){
        m1[i]	+= S_ip * m_p;
        v1[i] += S_ip * v_p; // ? need to be updated by mpm force?
      }else{
        m2[i] += S_ip * m_p;
        v2[i] += S_ip * v_p; // ? need to be updated by mpm force?
      }
    }
  }
	// compute contact force and update v_i_temp to v_i
	std::vector<float> m_cm(node_num);
	for(i = 1 ... node_num){
    if(m1[i] > 0 && m2[i] > 0){
      m_cm[i] = m1[i] + m2[i];
      v_cm[i] = (v1[i] * m1[i] + v2[i] * m2[i]) / (m1[i] + m2[i]);
      [n1, n2] = COMPUTE_NORMAL(m_p); // n1 = -n2
      
      [s_A, s_B] = COMPUTE_OTRH_BASIS(n1); // compute tangential direction
      // contact pair 1
      {
        if((v_cm[i] - v1[i]) * n1 > 0){ 
          f_nor = m1[i] * (v_cm[i] - v1[i]) / dt * n1;
          f_tan_A = m1[i] * (v_cm[i] - v1[i]) / dt * s_A;
          f_tan_B = m1[i] * (v_cm[i] - v1[i]) / dt * s_B;
          f_tan   = (f_tan_A * s_A + f_tan_B * s_B).norm();
          s				= (f_tan_A * s_A + f_tan_B * s_B).normalized();
          f_ct_1[i]  = f_nor * n1 + min(mu * abs(f_nor), abs(f_tan)) * f_tan * s;
        }
      }
      
      // contact pair 2
      {
        if((v_cm[i] - v1[i]) * n2 > 0){ 
          f_nor = m1[i] * (v_cm[i] - v1[i]) / dt * n2;
          f_tan_A = m1[i] * (v_cm[i] - v1[i]) / dt * s_A;
          f_tan_B = m1[i] * (v_cm[i] - v1[i]) / dt * s_B;
          f_tan   = (f_tan_A * s_A + f_tan_B * s_B).norm();
          s				= (f_tan_A * s_A + f_tan_B * s_B).normalized();
          f_ct_2[i]  = f_nor * n2 + min(mu * abs(f_nor), abs(f_tan)) * f_tan * s;
        }
      }
      
     	v_1[i] += f_ct_1[i] / m1[i] * dt;
      v_2[i] += f_ct_2[i] / m2[i] * dt;
      v[i] = (v1[i] * m1[i] + v2[i] * m2[i]) / (m1[i] + m2[i]);
    }
  }
```

